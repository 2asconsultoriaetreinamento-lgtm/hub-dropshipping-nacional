# Documento 4: Manual de Onboarding e Processo de Adesão
**Versão:** 1.0  
**Data:** Março/2026  

---

## 1. ONBOARDING DO FORNECEDOR (INDÚSTRIA)

O fornecedor é o pilar de estoque do sistema. O processo de adesão consiste em:

### 1.1 Cadastro e Dados Fiscais
- Preenchimento de Dados Cadastrais (CNPJ, IE, Endereço de Coleta).
- Upload do Logotipo para personalização (opcional).

### 1.2 Configuração Técnica (ERP)
- **Integração:** Inserção da API Key / Token do Bling ou Tiny.
- **Webhook:** Configuração da URL de callback fornecida pelo Hub no painel do ERP.
- **Mapeamento:** Seleção do depósito padrão para consulta de estoque.

### 1.3 Homologação de Produtos
- Importação inicial de catálogo.
- Validação de SKUs e preços de custo.

---

## 2. ONBOARDING DO LOJISTA (CANAL DE VENDA)

O lojista foca na tração de vendas e gestão do saldo.

### 2.1 Aceite de Termos e Riscos
- **Assinatura Digital:** Aceite obrigatório do "Termo de Ciência de Risco Operacional".

### 2.2 Configuração da Wallet (Carteira)
- **Depósito Inicial:** O sistema exige um saldo mínimo (ex: R$ 500,00) para ativação do canal.
- **Dados Bancários:** Para eventuais estornos.

---

## 3. CHECKLIST DE ATIVAÇÃO ("GO-LIVE")

O parceiro (Fornecedor ou Lojista) só terá o status `Active` após:
1.  [ ] API Key validada com sucesso.
2.  [ ] Webhook de teste recebido pelo Hub.
3.  [ ] Termo de contrato aceito.
4.  [ ] (Lojista) Saldo em Wallet > 0.

---

## 4. SUPORTE E TREINAMENTO
- Acesso à base de conhecimento (Documentação Técnica).
- Canal de suporte via chat interno para dúvidas de integração.
