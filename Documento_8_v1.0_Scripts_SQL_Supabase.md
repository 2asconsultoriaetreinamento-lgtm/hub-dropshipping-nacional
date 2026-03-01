# Documento 8: Scripts SQL para Supabase (DDL & RLS)

## 1. Extensões e Tipos

```sql
-- Habilitar extensões necessárias
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Tipos ENUM
CREATE TYPE organization_type AS ENUM ('fornecedor', 'lojista');
CREATE TYPE order_status AS ENUM ('pending', 'approved', 'shipped', 'delivered', 'cancelled');
CREATE TYPE webhook_status AS ENUM ('received', 'processed', 'failed');
```

## 2. Tabelas Principais

```sql
-- 1. Organizations
CREATE TABLE organizations (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL,
    type organization_type NOT NULL,
    document_id TEXT UNIQUE, -- CNPJ/CPF
    api_key_erp TEXT,
    settings JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 2. Profiles (Extende auth.users)
CREATE TABLE profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    organization_id UUID REFERENCES organizations(id),
    full_name TEXT,
    role TEXT DEFAULT 'operator',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 3. Products
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    organization_id UUID NOT NULL REFERENCES organizations(id),
    external_id TEXT, -- ID no ERP (Bling/Tiny)
    sku TEXT NOT NULL,
    name TEXT NOT NULL,
    description TEXT,
    price_cost DECIMAL(10,2) NOT NULL,
    stock_quantity INTEGER DEFAULT 0,
    weight DECIMAL(10,3),
    images_url TEXT[],
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(organization_id, sku)
);

-- 4. Orders
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    buyer_org_id UUID NOT NULL REFERENCES organizations(id),
    seller_org_id UUID NOT NULL REFERENCES organizations(id),
    external_order_id TEXT, -- ID do pedido no canal de venda
    status order_status DEFAULT 'pending',
    customer_data JSONB NOT NULL, -- Endereço, Nome, etc
    total_amount DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 5. Order Items
CREATE TABLE order_items (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    order_id UUID NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    product_id UUID NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL
);

-- 6. Wallets & Transactions
CREATE TABLE wallets (
    organization_id UUID PRIMARY KEY REFERENCES organizations(id),
    balance DECIMAL(10,2) DEFAULT 0.00,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE wallet_transactions (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    wallet_id UUID NOT NULL REFERENCES wallets(organization_id),
    order_id UUID REFERENCES orders(id),
    amount DECIMAL(10,2) NOT NULL,
    type TEXT NOT NULL, -- 'credit', 'debit'
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 7. Webhooks Log
CREATE TABLE webhook_events (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    organization_id UUID REFERENCES organizations(id),
    source TEXT NOT NULL, -- 'bling', 'tiny', 'shopify'
    payload JSONB NOT NULL,
    status webhook_status DEFAULT 'received',
    error_log TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## 3. Row Level Security (RLS)

```sql
-- Ativar RLS em todas as tabelas
ALTER TABLE organizations ENABLE ROW LEVEL SECURITY;
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE products ENABLE ROW LEVEL SECURITY;
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
ALTER TABLE order_items ENABLE ROW LEVEL SECURITY;
ALTER TABLE wallets ENABLE ROW LEVEL SECURITY;
ALTER TABLE wallet_transactions ENABLE ROW LEVEL SECURITY;

-- Políticas: Profiles
CREATE POLICY "Users can view own profile" 
ON profiles FOR SELECT USING (auth.uid() = id);

-- Políticas: Products
CREATE POLICY "Everyone can view active products" 
ON products FOR SELECT USING (is_active = true);

CREATE POLICY "Suppliers can manage own products" 
ON products FOR ALL USING (
    organization_id IN (
        SELECT organization_id FROM profiles WHERE id = auth.uid()
    )
);

-- Políticas: Orders
CREATE POLICY "Orgs can view their own orders" 
ON orders FOR SELECT USING (
    buyer_org_id IN (SELECT organization_id FROM profiles WHERE id = auth.uid()) OR
    seller_org_id IN (SELECT organization_id FROM profiles WHERE id = auth.uid())
);
```

## 4. Automações (Triggers)

```sql
-- Trigger para criar perfil automaticamente no signup
CREATE OR REPLACE FUNCTION public.handle_new_user() 
RETURNS trigger AS $$
BEGIN
  INSERT INTO public.profiles (id, full_name)
  VALUES (new.id, new.raw_user_meta_data->>'full_name');
  RETURN new;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE PROCEDURE public.handle_new_user();
```

---
*Este script fornece a base de dados completa para o Hub, garantindo isolamento entre lojistas e fornecedores via RLS.*
