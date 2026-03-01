# Especificação Técnica: Hub de Dropshipping Nacional
**Versão:** 3.6  
**Status:** Planejamento Avançado  

---

## 1. VISÃO E ESCOPO DO PROJETO

### 1.1 Objetivo do Sistema
Plataforma SaaS para centralizar e automatizar a operação de dropshipping entre fornecedores industriais e lojistas finais.

### 1.2 Glossário de Termos Principais
- **Hub:** A plataforma central (orquestrador).
- **Fornecedor:** Entidade que detém o estoque e emite a nota fiscal (NF-e).
- **Lojista:** O canal de venda que atrai o cliente final.
- **Wallet (Carteira):** Saldo pré-pago do lojista no Hub para liquidação automática de pedidos.

---

## 2. ARQUITETURA DE INTEGRAÇÃO (WEBHOOKS E APIs)

### 2.1 Webhooks do Fornecedor (ERP Bling/Tiny)
O sistema deve processar eventos em tempo real:
- **Estoque:** Atualização via `webhook push` do ERP para garantir sincronia imediata.
- **Status de Pedido:** Mudanças de status (Aprovado, Enviado, Entregue).
- **NF-e:** Recebimento da chave de acesso e XML assim que emitida.

### 2.2 Fluxo de Autenticação
- Integração via **OAuth 2.0 / Token** com emissores parceiros e ERPs no MVP.

---

## 3. FLUXOS OPERACIONAIS DETALHADOS

### 3.1 Fluxo de Venda e Pagamento
1. Pedido realizado no canal do Lojista.
2. Hub valida saldo na **Wallet** do Lojista.
3. Se houver saldo, o Hub envia o pedido ao ERP do Fornecedor.
4. Liquidação financeira: Divisão automática do valor entre fornecedor e Hub.

### 3.2 Estratégia de Faturamento (NF-e)
Adotada a **Opção A**: Emissão de uma única NF-e (Fornecedor -> Cliente Final) contendo todos os itens, simplificando a logística e tributação no MVP.

---

## 4. GESTÃO DE RISCO E CANCELAMENTOS

### 4.1 Onboarding de Canais
- Exigência do **Termo de Ciência de Risco** durante a adesão do canal.

### 4.2 Matriz de Cancelamento
- **Cenário A:** Cancelamento antes do faturamento (Estorno automático na Wallet).
- **Cenário B:** Cancelamento após faturamento (Necessita processo de logística reversa).

---

## 5. REQUISITOS Fiscais e Tributários
- Alertas críticos integrados para **ICMS-ST** e **DIFAL** baseados na origem/destino.
- Monitoramento de notas fiscais de devolução.
