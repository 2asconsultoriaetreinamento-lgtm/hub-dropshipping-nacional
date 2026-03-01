# Documento 2: Arquitetura e Modelagem de Dados
**Versão:** 1.0  
**Data:** Março/2026  

---

## 1. ESTRATÉGIA MULTI-TENANT

Para garantir o isolamento total dos dados e escalabilidade no Supabase, utilizaremos:
- **Tenant ID:** Cada tabela terá uma coluna `organization_id` (UUID).
- **RLS (Row Level Security):** Políticas globais que filtram registros baseados no `auth.uid()` e na associação do usuário à organização.

---

## 2. MODELO DE DADOS (ENTIDADES PRINCIPAIS)

### 2.1 Gestão de Organizações e Perfis
- **organizations:** Armazena dados da empresa (Fornecedor ou Lojista), tipo de entidade e chaves de API.
- **profiles:** Extensão da tabela `auth.users`, contendo o `organization_id` e o `role` (Admin, Operador).

### 2.2 Catálogo de Produtos
- **products:** Centraliza os itens disponíveis para dropshipping.
  - Campos: `id`, `organization_id` (Fornecedor), `external_id` (ERP), `sku`, `name`, `description`, `price_cost`, `stock_quantity`, `weight`, `dimensions`, `images_url`.

### 2.3 Operações e Pedidos
- **orders:** Registra a intenção de compra.
  - Campos: `id`, `buyer_org_id` (Lojista), `seller_org_id` (Fornecedor), `customer_data` (JSONB), `status` (Pending, Approved, Shipped, Delivered, Cancelled).
- **order_items:** Itens individuais vinculados ao pedido.

### 2.4 Financeiro e Wallet
- **wallets:** Saldo atualizado do lojista.
- **wallet_transactions:** Histórico imutável de entradas (recargas) e saídas (pagamento de pedidos).

---

## 3. INTEGRAÇÃO COM ERP (Bling/Tiny)

### 3.1 Webhook Incoming
Tabela: **webhook_events**
- Registra o payload bruto recebido.
- Status: `received`, `processed`, `failed`.

### 3.2 Mapeamento de Status
- `ERP: Em aberto` -> `HUB: Pending`
- `ERP: Atendido` -> `HUB: Shipped`
- `ERP: Cancelado` -> `HUB: Cancelled`

---

## 4. SEGURANÇA (RLS)

- **Leitura de Produtos:** Lojistas podem ler todos os produtos ativos; Fornecedores leem apenas os seus.
- **Gestão de Pedidos:** Lojistas veem pedidos onde são `buyer`; Fornecedores veem onde são `seller`.
- **Wallet:** Acesso exclusivo ao próprio saldo da organização.
