# Documento 10: Roadmap de Desenvolvimento (Sprint 1)

Este documento organiza a execução prática do MVP do Hub de Dropshipping Nacional, priorizando o Onboarding e a Infraestrutura Base.

## Fase 1: Setup de Infraestrutura (Dia 1-2)

### 1.1 Configuração do Supabase
- [ ] Criar novo projeto no Supabase.
- [ ] Rodar os scripts SQL do **Documento 8** no SQL Editor.
- [ ] Configurar o Supabase Auth (Habilitar Google Provider se desejar).

### 1.2 Inicialização do Frontend (Lovable)
- [ ] Criar novo projeto no Lovable.ai.
- [ ] Conectar o projeto ao repositório GitHub e ao Supabase recém-criado.
- [ ] Definir as cores globais e tema base (Documento 9).

## Fase 2: Onboarding de Usuários (Dia 3-5)

### 2.1 Fluxo de Cadastro
- [ ] Implementar Tela 1.1 (Signup com seletor Lojista/Fornecedor).
- [ ] Validar a criação automática do registro na tabela `profiles` via Trigger.

### 2.2 Termo de Ciência de Risco
- [ ] Implementar Tela 1.2 (Acordo de Risco).
- [ ] Garantir que o status de `onboarding_completed` só mude após o aceite.

## Fase 3: Integração Mínima Viável - ERP (Dia 6-10)

### 3.1 Painel do Fornecedor (Setup)
- [ ] Criar tela para input da API Key do Bling/Tiny.
- [ ] Exibir a URL de Webhook para o fornecedor.

### 3.2 Sincronização de Estoque (Edge Functions)
- [ ] Criar a Edge Function `process-webhook` para receber dados do ERP.
- [ ] Testar a atualização automática da tabela `products` ao alterar estoque no ERP.

## Fase 4: Wallet e Ativação (Dia 11-15)

### 4.1 Wallet Inicial
- [ ] Criar tela de saldo pré-pago.
- [ ] Implementar mockup de recarga (PIX/Depósito) para testes.

---

### Critérios de Sucesso (Fim da Sprint 1):
1. Usuário consegue se cadastrar e aceitar o termo de risco.
2. Fornecedor consegue conectar o ERP e o sistema recebe o estoque.
3. Lojista consegue visualizar os produtos importados do fornecedor.

*Este roadmap garante que os riscos técnicos e de negócio (onboarding/estoque) sejam resolvidos logo no início do projeto.*
