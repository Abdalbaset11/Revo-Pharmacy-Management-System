# API Specification & Contract — Revo Pharmacy Management System

This document specifies the database query and mutation contract for the Revo frontend app using the Supabase client library. In a contract-first development flow, the frontend relies on these exact queries, inputs, and schemas.

---

## 1. Authentication Layer

### Sign In
- **Method**: `supabase.auth.signInWithPassword({ email, password })`
- **Input Validation (Zod)**:
  ```typescript
  const signInSchema = z.object({
    email: z.string().email("Invalid email address"),
    password: z.string().min(6, "Password must be at least 6 characters")
  });
  ```
- **Response**:
  ```json
  {
    "user": {
      "id": "uuid",
      "email": "user@revo.com",
      "role": "authenticated"
    },
    "session": {
      "access_token": "jwt_string",
      "refresh_token": "uuid"
    }
  }
  ```

---

## 2. Product Catalog

### Get Products (Paginated & Filtered)
- **Method**:
  ```typescript
  supabase
    .from('products')
    .select('id, trade_name, scientific_name, barcode, category, purchase_price, selling_price, expiry_date, min_stock, stock_qty_lowest_unit')
    .ilike('trade_name', `%${search}%`)
    .range(start, end)
    .order('trade_name', { ascending: true })
  ```
- **Response**: Array of product objects.

### Create Product
- **Method**:
  ```typescript
  supabase
    .from('products')
    .insert([productData])
    .select()
  ```
- **Validation (Zod)**:
  ```typescript
  const productSchema = z.object({
    trade_name: z.string().min(1, "Trade name is required"),
    scientific_name: z.string().optional(),
    barcode: z.string().optional(),
    category: z.string().optional(),
    purchase_price: z.number().positive("Must be a positive number"),
    selling_price: z.number().positive("Must be a positive number"),
    expiry_date: z.string().regex(/^\d{4}-\d{2}-\d{2}$/, "Invalid date format"),
    min_stock: z.number().int().nonnegative().default(10),
    unit_name_primary: z.string().min(1),
    unit_name_secondary: z.string().optional(),
    unit_name_tertiary: z.string().optional(),
    conversion_rate_1: z.number().int().positive().optional(),
    conversion_rate_2: z.number().int().positive().optional()
  });
  ```

---

## 3. POS & Transactions

### Create Sales Invoice (Single Transaction Block)
Transactions in Supabase can be grouped using database functions (RPC) to guarantee ACID integrity.

- **Method**: `supabase.rpc('create_sales_invoice', { invoice_payload })`
- **Payload Schema (Zod)**:
  ```typescript
  const salesInvoiceSchema = z.object({
    customer_id: z.string().uuid().optional(),
    insurance_company_id: z.string().uuid().optional(),
    total_amount: z.number().positive(),
    discount: z.number().nonnegative(),
    insurance_coverage: z.number().nonnegative(),
    patient_copay: z.number().nonnegative(),
    payment_type: z.enum(['Cash', 'Card', 'Debt', 'Split']),
    amount_paid_cash: z.number().nonnegative(),
    amount_paid_card: z.number().nonnegative(),
    items: z.array(z.object({
      product_id: z.string().uuid(),
      unit_sold: z.enum(['Box', 'Strip', 'Piece']),
      qty: z.number().int().positive(),
      unit_price: z.number().positive(),
      total_price: z.number().positive()
    })).min(1, "At least one item required")
  });
  ```

### Behind the Scenes RPC Logic:
```sql
CREATE OR REPLACE FUNCTION create_sales_invoice(invoice_payload JSONB)
RETURNS UUID AS $$
DECLARE
  new_invoice_id UUID;
  item_record RECORD;
BEGIN
  -- 1. Insert into sales_invoices
  INSERT INTO public.sales_invoices (...) VALUES (...) RETURNING id INTO new_invoice_id;
  
  -- 2. Insert invoice items & Deduct stock
  FOR item_record IN SELECT * FROM jsonb_to_recordset(invoice_payload->'items') AS x(...) LOOP
    INSERT INTO public.sales_items (sales_invoice_id, product_id, unit_sold, qty, unit_price, total_price)
    VALUES (new_invoice_id, item_record.product_id, item_record.unit_sold, item_record.qty, item_record.unit_price, item_record.total_price);
    
    -- Update product stock level (converting box/strip/piece to lowest units)
    UPDATE public.products 
    SET stock_qty_lowest_unit = stock_qty_lowest_unit - calculate_lowest_units(item_record.product_id, item_record.unit_sold, item_record.qty)
    WHERE id = item_record.product_id;
  END LOOP;
  
  RETURN new_invoice_id;
END;
$$ LANGUAGE plpgsql;
```

---

## 4. Expenses Management

### Create Expense Record
- **Method**:
  ```typescript
  supabase
    .from('expenses')
    .insert([expenseData])
    .select()
  ```
- **Validation (Zod)**:
  ```typescript
  const expenseSchema = z.object({
    category: z.enum(['Rent', 'Electricity', 'Salaries', 'Water', 'Internet', 'Maintenance', 'Other']),
    amount: z.number().positive("Amount must be positive"),
    description: z.string().optional()
  });
  ```
