# Documento 9: Guia de Prototipagem Lovable (Detalhamento de Telas)

Este documento serve como guia para a construção das interfaces no Lovable.ai, garantindo que a jornada do usuário e as regras de negócio sejam respeitadas.

## 1. Fluxo de Onboarding (Lojista)

### Tela 1.1: Cadastro e Identificação
- **Elementos:** Form de nome, email, senha e Tipo de Empresa (Lojista/Fornecedor).
- **Ação Lovable:** "Create a signup screen with a toggle for 'Retailer' or 'Supplier'. Use Radix UI for the toggle."

### Tela 1.2: Termo de Ciência de Risco (Crítico)
- **Elementos:** Texto legal (Documento 4) e Checkbox de aceite obrigatório.
- **Regra:** O botão "Continuar" só habilita após o check.
- **Ação Lovable:** "Display a scrollable risk disclosure agreement. Enable the 'Accept and Continue' button only after the checkbox is checked."

### Tela 1.3: Configuração de Wallet Inicial
- **Elementos:** Exibição de QR Code para primeiro depósito (Saldo Pré-pago).
- **Ação Lovable:** "Create a wallet setup screen showing a mock QR Code for PIX deposit. Include a 'Pending Confirmation' status badge."

## 2. Dashboard do Lojista

### Tela 2.1: Catálogo de Produtos (Dropshipping)
- **Elementos:** Grid de cards com imagem, SKU, Preço de Custo, Estoque Atual e botão "Vender este item".
- **Ação Lovable:** "Design a product catalog grid. Each card should show stock status from Supabase and a 'Connect to Store' button."

### Tela 2.2: Gestão de Pedidos
- **Elementos:** Tabela com filtros (Aguardando Pagamento, Enviado, Cancelado).
- **Ação Lovable:** "Build an order management table with status tags: Pending (yellow), Shipped (blue), Cancelled (red)."

## 3. Painel do Fornecedor

### Tela 3.1: Configuração de Integração Bling/Tiny
- **Elementos:** Campos para API Key e URL de Webhook do Hub.
- **Ação Lovable:** "Create an ERP integration settings page with a field for API Key and a read-only field showing the Webhook URL to be copied to Bling."

### Tela 3.2: Monitor de Sincronização
- **Elementos:** Lista de logs de webhook recentes.
- **Ação Lovable:** "Create a live feed of webhook events showing status (Success/Error) and the timestamp of the last stock update."

## 4. Estilo Visual (Branding)
- **Primary Color:** Azul Marinho (#1E293B) - Confiança e Profissionalismo.
- **Accent Color:** Verde Esmeralda (#10B981) - Sucesso e Dinheiro.
- **Component Library:** Shadcn/ui (Tailwind CSS).

---
*Este guia deve ser utilizado para instruir o Lovable.ai na geração dos componentes React para o MVP.*
