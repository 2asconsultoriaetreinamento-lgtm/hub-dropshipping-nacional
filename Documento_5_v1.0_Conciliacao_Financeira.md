# Documento 5: Fluxo de Conciliação Financeira e Repasses
**Versão:** 1.0  
**Data:** Março/2026  

---

## 1. MODELO FINANCEIRO (TRIANGULAÇÃO)

O Hub atua como o custodiante dos valores transacionados. A lógica de divisão do valor segue o fluxo:
1.  **Valor Pago pelo Lojista:** (Preço de Custo do Produto + Taxa do Hub + Frete).
2.  **Wallet (Débito):** O valor total é debitado do saldo pré-pago do Lojista no momento da aprovação do pedido.
3.  **Retenção do Hub:** A taxa de serviço do Hub é separada internamente.
4.  **Repasse ao Fornecedor:** O valor líquido (Custo + Frete) é provisionado para o fornecedor.

---

## 2. REGRAS DE REPASSE (FORNECEDOR)

### 2.1 Prazos de Liquidação
- **Provisionamento:** Ocorre no momento do faturamento (emissão da NF-e).
- **Disponibilidade para Saque:** Definido conforme contrato (ex: D+7 após a entrega confirmada ou D+15 após faturamento).

### 2.2 Relatório de Fechamento
Mensalmente, o sistema gera um "Extrato de Repasse" contendo:
- Lista de pedidos faturados no período.
- Deduções de cancelamentos ou devoluções.
- Valor total a ser transferido para a conta bancária do fornecedor.

---

## 3. CONCILIAÇÃO DE CANCELAMENTOS

- **Cancelamento Pré-Faturamento:** Estorno imediato e integral do valor na Wallet do Lojista.
- **Cancelamento Pós-Faturamento:** O valor só é estornado após a confirmação da entrada da mercadoria de volta ao estoque do fornecedor (Logística Reversa).

---

## 4. ASPECTOS FISCAIS DO HUB

O Hub não emite NF-e dos produtos, mas sim uma **NFS-e (Nota Fiscal de Serviço)** referente à taxa de intermediação cobrada dos parceiros.
- Periodicidade: Mensal.
- Base de Cálculo: Soma de todas as taxas de intermediação retidas no mês.
