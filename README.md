# 📦 Controle de NFs Importados - SUL x CD Rio

## Visão Geral

Com o objetivo de simplificar e aumentar a confiabilidade do acompanhamento das transferências de produtos importados entre o **SUL** e o **CD Rio**, o controle passa a ser realizado através de uma **planilha Google integrada a automações**, substituindo a antiga lista do SharePoint.

> A lista do SharePoint permanecerá disponível apenas para consultas históricas.

---

# 🔄 Fluxo do Processo

```text
Faturamento SUL
       ↓
Cadastro da NF
       ↓
Atualização Automática da Planilha
       ↓
Acompanhamento Logístico
       ↓
Recebimento no CD Rio
       ↓
Confirmação de Recebimento
```

---

# 1. Cadastro da Nota Fiscal (Faturamento)

## Objetivo

Registrar a Nota Fiscal no momento da expedição para iniciar o monitoramento automático da carga.

## Link de Cadastro

https://n8n.somalabs.com.br/form/a1272b6f-3ffa-40bb-b713-6707dec59836

## Como preencher

### Opção 1

Informar:

- Número da NF
- Série da NF

### Opção 2

Informar:

- Chave de acesso da NF

A chave pode ser capturada diretamente do DANFE utilizando um leitor de código de barras.

### Informação obrigatória

Além dos dados da NF, informar:

- Número do carro / veículo

## Validações automáticas

Após o preenchimento, o sistema consulta a nota e exibe:

- Referência
- Descrição
- Marca
- Canal
- Quantidade
- Valor da NF

## Possíveis mensagens de erro

| Situação | Ação |
|-----------|--------|
| Nota não localizada | Revisar NF/Série ou informar a chave |
| Nota já cadastrada | Verificar cadastro existente |
| Nota localizada em mais de uma filial | Utilizar a chave de acesso |

---

# 2. Confirmação de Recebimento (CD Rio)

## Objetivo

Registrar o recebimento físico da carga no CD Rio.

## Link de Recebimento

https://n8n.somalabs.com.br/form/9c38dd32-8273-4c09-93b9-483d265f0540

## Como preencher

Informar:

- Número da NF
- Série

ou

- Chave de acesso

### Divergências

Caso existam faltas ou sobras, registrar no campo **Observação**.

Exemplos:

```text
FALTA 1 M

SOBRA 2 P

FALTA 3 G / SOBRA 1 M
```

## Registro automático

O sistema grava automaticamente:

- Data do recebimento
- Horário do recebimento

---

# 3. Acompanhamento Operacional

## Planilha de Controle

https://docs.google.com/spreadsheets/d/1IR2x958ruchBiYUOOAECPMXTxhrPR5Js2KaVuTJbFSo/edit?usp=sharing

## Atualizações automáticas

As informações são atualizadas automaticamente:

| Frequência | Horário |
|------------|----------|
| Segunda a sábado | 07h às 19h |
| Intervalo | A cada 1 hora |

## Informações atualizadas pelo sistema

- Agendamento
- Conferência no SUL
- Romaneio
- Transportadora
- Placa
- Motorista
- Embarque
- Previsão de chegada
- Entrada Fiscal

## Preenchimento da planilha

### 🟩 Cabeçalhos verdes

Campos destinados ao preenchimento manual pelos usuários.

### ⬜ Cabeçalhos cinza

Campos preenchidos automaticamente pelo sistema.

Esses campos estão protegidos e não devem ser alterados.

## Tratamento de inconsistências

Quando houver algum problema relacionado à nota, a justificativa será apresentada automaticamente na coluna:

### MENSAGEM

Exemplos:

- Nota não localizada
- Nota duplicada
- Divergência cadastral
- Falha de integração

---

# ⚠️ Regras Importantes

## Não altere a coluna IDENTIFICACAO

A coluna **IDENTIFICACAO** é utilizada pelas automações para localizar e atualizar os registros corretamente.

Alterações podem causar:

- Falhas de atualização
- Perda de rastreabilidade
- Erros no processamento automático

---

## Não altere a estrutura da planilha

Não é permitido:

- Alterar formatos das colunas
- Excluir linhas
- Criar novas colunas
- Renomear colunas
- Alterar a posição das colunas

---

# 🔒 Segurança de Acesso

Os acessos aos formulários e à planilha são protegidos por autenticação.

Usuário e senha serão enviados separadamente.

### Importante

Não compartilhe:

- Links dos formulários
- Link da planilha
- Usuário
- Senha

com pessoas externas ao processo.

---

# 📋 Responsabilidades

## Faturamento SUL

Responsável por:

- Registrar a NF
- Informar corretamente os dados do transporte
- Garantir que a nota esteja disponível para acompanhamento

## CD Rio

Responsável por:

- Confirmar o recebimento
- Registrar divergências
- Garantir a rastreabilidade da entrega

## Sistema

Responsável por:

- Atualizar informações automaticamente
- Validar os dados das NFs
- Registrar data e horário das movimentações
- Apontar inconsistências operacionais

---

# ✅ Benefícios da Solução

- Eliminação do controle manual via SharePoint
- Atualização automática dos dados logísticos
- Redução de erros operacionais
- Maior rastreabilidade das cargas
- Histórico centralizado de movimentações
- Menor retrabalho operacional
- Maior visibilidade para as áreas envolvidas

---

# 🚀 Resultado Esperado

Garantir o acompanhamento integral das Notas Fiscais de importados entre o SUL e o CD Rio, com informações atualizadas automaticamente, rastreabilidade ponta a ponta e redução significativa de controles manuais.



### Nome do Fluxo N8N: *Torre de Controle Importados SUL x RIO - Google Sheets*
