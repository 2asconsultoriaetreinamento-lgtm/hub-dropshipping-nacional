# Documento 6: Fluxo de Telas e Jornada do Usuário (UI/UX Specification)
**Versão:** 1.0  
**Data:** Março/2026  

---

## 1. MAPA DE NAVEGAÇÃO GERAL

O Hub possui três portas de entrada principais:
- **Login Centralizado:** Autenticação via email/senha (Supabase Auth).
- **Dashboard Principal:** Hub de entrada após autenticação.
- **Portais Específicos:** Fornecedor vs. Lojista (diferentes interfaces).

---

## 2. FLUXO DO FORNECEDOR (Indústria)

### 2.1 Tela: Login & Onboarding
- Campo: Email.
- Campo: Senha.
- Link: "Cadastro de Novo Fornecedor".

### 2.2 Tela: Dashboard do Fornecedor
- **Seção Superior:**
  - Nome da Empresa.
  - Saldo a Receber (D+7 / D+15).
- **Menu Lateral:**
  - Meus Produtos.
  - Meus Pedidos.
  - Extratos de Repasse.
  - Configurações.

### 2.3 Tela: Meus Produtos
- Tabela com: SKU | Nome | Estoque Atual | Preço Custo | Ações.
- Botão: "+ Adicionar Produto (importar do ERP)".
- Sincronização automática via webhook em tempo real.

### 2.4 Tela: Meus Pedidos
- Tabela com: ID Pedido | Lojista | Status | Data Criação | Ações.
- Status: Pendente → Aprovado → Faturado → Enviado → Entregue.
- Botão: "Visualizar Detalhes" (mostra Nota Fiscal e rastreio).

---

## 3. FLUXO DO LOJISTA (Canal de Venda)

### 3.1 Tela: Login & Onboarding
- Aceite digital do "Termo de Ciência de Risco".
- Configuração da Wallet (depósito inicial obrigatório).

### 3.2 Tela: Dashboard do Lojista
- **Seção de Saldo:**
  - Saldo Atual na Wallet.
  - Botão: "+ Recarregar Wallet".
  - Histórico de Transações.
- **Menu Lateral:**
  - Catálogo (listar produtos disponíveis).
  - Meus Pedidos.
  - Faturamento & NF-e.
  - Configurações.

### 3.3 Tela: Catálogo de Produtos
- Listagem de produtos com filtros (Fornecedor, Categoria, Disponibilidade).
- Card de Produto: Imagem | Nome | Preço Custo | Preço Final | "Adicionar ao Carrinho".
- Carrinho flutuante (ícone com contador).

### 3.4 Tela: Checkout
- Resumo de Itens.
- Endereço de Entrega (Cliente Final).
- Validação de Saldo na Wallet.
- Botão: "Confirmar Pedido" (se saldo suficiente).

---

## 4. COMPONENTES REUTILIZÁVEIS (Design System)

- **Botões:** Primary (CTA), Secondary, Danger.
- **Formulários:** Inputs, Selects, Checkboxes, Campos de Data.
- **Notificações:** Toast (sucesso, erro, aviso).
- **Modal:** Confirmação de ações críticas.
- **Tabelas:** Ordenação, Paginação, Filtros.

---

## 5. FLUXO DE INTEGRAÇÃO (APIs React/TypeScript)

- Fetch de Produtos: GET `/api/products?supplier_id=X`.
- Criar Pedido: POST `/api/orders` (com validação de wallet).
- Atualizar Estoque: Webhook listener (socket.io ou polling).

---

## 6. SEGURANÇA E ACESSIBILIDADE

- **RLS:** Cada usuário vê apenas seus dados (supplier_id ou buyer_id).
- **WCAG:** Contraste de cores, tamanho de fonte, navegação por teclado.
