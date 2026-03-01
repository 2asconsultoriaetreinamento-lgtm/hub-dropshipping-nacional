# Documento 10: Roadmap de Desenvolvimento (Sprint 1)
## Versão 1.0 - Roadmap da Execução do MVP do Hub de Dropshipping Nacional

**Data de Criação:** 2025-01-17
**Versão:** 1.0
**Status:** Em Execução
**Responsável:** Equipe de Desenvolvimento - 2As Consultoria

---

## Sumário Executivo

Este documento estrutura a execução prática do MVP (Minimum Viable Product) do **Hub de Dropshipping Nacional**, focando nos primeiros 15 dias de desenvolvimento. O objetivo primário é estabelecer uma infraestrutura robusta, validar o fluxo de onboarding de parceiros e implementar a integração básica com sistemas ERP.

**Objetivos Principais da Sprint 1:**
1. Setup da infraestrutura backend (Supabase) com schemas e segurança RLS.
2. Validação do fluxo de onboarding legal (Termo de Ciência de Risco).
3. Integração funcional com ERP (Bling/Tiny) para sincronização de estoque.
4. Implementação do Wallet/Saldo pré-pago para lojistas.
5. Prototipagem das telas principais no Lovable.

---

## Fase 1: Setup de Infraestrutura (Dias 1-2)

### 1.1 Configuração do Supabase
- [ ] Criar novo projeto no Supabase.
- [ ] Rodar scripts SQL do **Documento 8** (Tabelas, RLS, Triggers).
- [ ] Configurar Autenticação (Google/Email).
- [ ] Habilitar Edge Functions runtime.

### 1.2 Inicialização do Frontend (Lovable)
- [ ] Criar projeto no Lovable.ai e conectar ao GitHub.
- [ ] Definir sistema de cores e tema base (Documento 9).
- [ ] Configurar Supabase Client com variáveis de ambiente.

---

## Fase 2: Onboarding de Usuários (Dias 3-5)

### 2.1 Fluxo de Cadastro e Perfil
- [ ] Implementar Tela 1.1: Signup com seletor Lojista/Fornecedor.
- [ ] Validar criação automática do perfil via Trigger no banco.

### 2.2 Termo de Ciência de Risco
- [ ] Implementar Tela 1.2: Termo de Risco com checkbox de aceite.
- [ ] Bloquear acesso ao sistema até que o termo seja aceito.
- [ ] Registrar log de aceite (Audit Log) para conformidade legal.

---

## Fase 3: Integração Mínima Viável - ERP (Dias 6-10)

### 3.1 Painel do Fornecedor (API Setup)
- [ ] Criar tela para input da API Key (Bling/Tiny).
- [ ] Exibir URL de Webhook única para o fornecedor.

### 3.2 Sincronização de Estoque
- [ ] Desenvolver Edge Function para processar Webhooks de estoque.
- [ ] Testar atualização automática da tabela `products` ao alterar estoque no ERP.

---

## Fase 4: Wallet e Ativação (Dias 11-15)

### 4.1 Wallet Inicial
- [ ] Criar tela de saldo e histórico de transações.
- [ ] Implementar mockup de recarga via PIX para validação do fluxo financeiro.

---

## Resumo de Entregáveis da Sprint 1

| Categoria | Entregável | Status Esperado |
| :--- | :--- | :--- |
| **Infra** | Supabase configurado com RLS | 100% |
| **Legal** | Fluxo de Termo de Risco funcional | 100% |
| **ERP** | Sincronização de estoque via Webhook | 100% |
| **Financeiro** | Painel de Wallet com saldo | 100% |
| **Frontend** | 5 telas principais prototipadas | 100% |

## Próximos Passos (Recomendação)
1. **Sprint 2:** Focar no Fluxo de Pedidos e Checkout com reserva de saldo.
2. **Sprint 3:** Integração logística (Melhor Envio) e Notas Fiscais.

---
**Nota:** Este documento deve ser atualizado ao final de cada milestone atingido.
