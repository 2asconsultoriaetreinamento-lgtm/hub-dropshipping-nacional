# Documento 7: Stack Técnico e Arquitetura de Deployment

## 1. Stack Tecnológica (MVP)

### 1.1 Frontend
- **Framework:** React + Vite (via Lovable.ai para prototipagem rápida).
- **Linguagem:** TypeScript (Strict Mode).
- **UI/UX:** Tailwind CSS + Radix UI + Lucide React.
- **Gerenciamento de Estado:** React Query (TanStack Query) para sync com server e cache.
- **Formulários:** React Hook Form + Zod (validação).

### 1.2 Backend & Infraestrutura (BaaS)
- **Plataforma:** Supabase.
- **Banco de Dados:** PostgreSQL (Multi-tenant via RLS - Row Level Security).
- **Autenticação:** Supabase Auth (OAuth com Google/GitHub e Email/Senha).
- **Storage:** Supabase Storage para notas fiscais, logos e arquivos de onboarding.
- **Edge Functions:** Deno (TypeScript) para lógica pesada e webhooks de terceiros.

### 1.3 Integrações de Terceiros
- **ERP Parceiro:** API REST (OAuth2/Token).
- **Checkout/Gateway:** Stripe ou Pagar.me (Integração via Webhooks).
- **E-commerce:** Shopify, WooCommerce, Nuvemshop (Conectores via Webhook/API).

## 2. Arquitetura de Dados e Segurança

### 2.1 Multi-tenancy (RLS)
- Cada tabela (orders, products, users) possui uma coluna `organization_id`.
- Políticas de RLS garantem que um Lojista nunca acesse dados de outro Lojista ou do Fornecedor.

### 2.2 Filas e Processamento Assíncrono
- Uso de `pg_net` ou Edge Functions para disparar webhooks de atualização de estoque para os canais de venda sem bloquear o banco.

## 3. Fluxo de Deployment e CI/CD

### 3.1 Ambientes
- **Development:** Desenvolvimento local e branches de feature.
- **Staging:** Branch `develop` com deploy automático para teste interno.
- **Production:** Branch `main` com deploy para o ambiente real.

### 3.2 Pipeline (GitHub Actions)
1. **Linting & Type Check:** Verificação de TypeScript e ESLint.
2. **Build:** Geração do bundle estático.
3. **Deploy Frontend:** Vercel ou Netlify (integrado ao Lovable/GitHub).
4. **Deploy Backend:** Supabase CLI para migrações de banco e edge functions.

## 4. Monitoramento e Observabilidade
- **Sentry:** Monitoramento de erros em tempo real.
- **Postgres Logs:** Auditoria de performance de queries complexas.
- **Custom Logs:** Tabela de `audit_logs` para rastrear mudanças críticas (preços, estoque).

---
*Este documento define os alicerces técnicos para garantir escalabilidade e segurança no MVP do Hub de Dropshipping Nacional.*
