# Documento 3: Manual de Integração e Webhooks (ERP Bling/Tiny)
**Versão:** 1.0  
**Data:** Março/2026  

---

## 1. SINCRONIZAÇÃO DE ESTOQUE (ERP -> HUB)

O estoque deve ser atualizado via **Webhook Push** para garantir latência zero.
- **Evento:** `estoque.atualizado`
- **Lógica de Processamento:**
  1. Recebe `external_id` (ID do produto no ERP).
  2. Localiza o produto na tabela `products` do Hub.
  3. Atualiza `stock_quantity`.
  4. Dispara notificação para canais de venda (Lojistas) caso o estoque chegue a zero.

---

## 2. ENVIO DE PEDIDOS (HUB -> ERP)

Assim que um pedido é aprovado no Hub (saldo validado na Wallet), o Hub realiza um **POST** na API do ERP do Fornecedor.
- **Endpoint:** `/pedidos`
- **Dados Obrigatórios:**
  - Dados do Cliente (Nome, CPF/CNPJ, Endereço).
  - Itens (SKU ERP, Quantidade, Valor Unitário).
  - Observação: "Pedido via Hub Dropshipping Nacional - ID: {hub_order_id}".

---

## 3. MONITORAMENTO DE STATUS E NF-E

O Hub deve escutar webhooks de mudança de situação do pedido no ERP:
- **Status 'Atendido' (Faturado):**
  - O Hub captura a **Chave de Acesso da NF-e**.
  - O Hub armazena o link/XML da nota para consulta do lojista.
  - O status do pedido no Hub muda para `Shipped`.

---

## 4. TRATAMENTO DE EXCEÇÕES E LOGS

### 4.1 Retentativas (Retry Policy)
- Se a API do ERP retornar erro (5xx), o Hub deve tentar novamente em: 5min, 15min, 1h.
- Após 3 falhas, o pedido deve ser marcado como `Error - Manual Intervention Required`.

### 4.2 Logs de Auditoria
- Todos os payloads (Request/Response) devem ser salvos na tabela `webhook_logs` por 90 dias.
