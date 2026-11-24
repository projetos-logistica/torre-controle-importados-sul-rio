<<<<<<< HEAD
# Torre de Controle de Importados | SUL → CD Rio

Aplicação interna para acompanhar as notas fiscais (NFs) de produtos importados desde o faturamento no SUL até o recebimento físico e a entrada fiscal no CD Rio.

O controle deixa de ser feito na lista do SharePoint e passa a ser feito em uma **tela web** publicada pelo n8n, com os dados gravados em uma planilha Google e enriquecidos automaticamente pelo SQL Server.

> A lista antiga do SharePoint permanece disponível **somente para consulta histórica**.

---

## 1. Visão geral

| Item | Descrição |
|---|---|
| Acesso | `https://n8n.somalabs.com.br/webhook/torre-importados/app` |
| Base de dados operacional | Google Sheets, aba `CONTROLE` |
| Fonte dos dados automáticos | SQL Server (faturamento, romaneios, recebimento agendado, entradas) |
| Orquestração | n8n (self-hosted, versão 2.4.8) |
| Autenticação | Usuário e senha, com sessão válida por 8 horas |

### O que a solução permite

- Cadastrar NFs em lote, com validação automática no faturamento;
- Acompanhar a situação de cada NF (aguardando embarque, em trânsito, recebida no Rio);
- Registrar o recebimento físico e as divergências no CD Rio;
- Consultar e filtrar por NF, carro, chave, marca ou referência;
- Ter rastreabilidade ponta a ponta, com data e hora de cada atualização.

### Fluxo do processo

```
Faturamento SUL → Cadastro da NF → Enriquecimento automático (SQL)
→ Acompanhamento logístico → Recebimento no CD Rio → Entrada fiscal
=======
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
>>>>>>> e24ef37cf6b9c17628168fcbd6e4f73fd0c69ca8
```

---

<<<<<<< HEAD
## 2. Guia de uso

### 2.1 Acesso

1. Abra o link de acesso da tabela acima. Use sempre o endereço `/webhook/`, **nunca** o `/webhook-test/`.
2. Informe usuário e senha. As credenciais são enviadas separadamente pelo responsável pelo processo.
3. A sessão expira em 8 horas ou ao clicar em **Sair**.

### 2.2 Cadastro de NFs (Faturamento SUL)

1. Clique em **＋ Cadastrar NFs**.
2. Informe:
   - **Carro**: identificação do veículo (ex.: `182-26`);
   - **Série**: série das NFs do lote (ex.: `12`);
   - **Notas fiscais**: uma por linha. Zeros à esquerda são completados automaticamente (`27607` → `000027607`).
3. Clique em **Adicionar notas à tabela**.

O sistema consulta cada NF no faturamento e preenche automaticamente: chave NF-e, marca, referência, descrição do material, canal, quantidade e valor.

**O lote é tudo ou nada:** se qualquer NF tiver problema, **nenhuma** é gravada, e a tela lista o motivo de cada recusa.

| Mensagem | Ação |
|---|---|
| Não localizada no faturamento | Revisar o número da NF e a série |
| Já cadastrada / chave já cadastrada | Conferir o registro existente na tela |
| Localizada em mais de uma filial | Acionar o time de sistemas para cadastro pela chave |
| Existem NFs repetidas no lote | Remover as duplicidades da lista |

Limite: 1 a 100 NFs por lote.

### 2.3 Recebimento (CD Rio)

1. Localize a NF pela busca.
2. Marque a caixa **Recebido Rio** e confirme na janela de confirmação.
3. A data e a hora do recebimento são gravadas automaticamente.

> ⚠️ O recebimento confirmado **não pode ser desfeito** pela tela.

**Divergências:** registre no campo **Observação** (até 500 caracteres). Exemplos:

```
FALTA 1 M
SOBRA 2 P
FALTA 3 G / SOBRA 1 M
```

### 2.4 Campos editáveis

| Cor do cabeçalho | Significado |
|---|---|
| 🟩 Verde | Preenchimento operacional: **Carro**, **Recebido Rio**, **Observação** |
| ⬜ Cinza | Dados automáticos do sistema; não editáveis |

Toda alteração é gravada imediatamente na planilha e confirmada na faixa no topo da tela.

### 2.5 Situação da NF

| Situação | Regra |
|---|---|
| Aguardando embarque | Sem data de embarque |
| Em trânsito | Com data de embarque, sem recebimento e sem entrada fiscal |
| Recebido no Rio | Recebimento confirmado na tela **ou** entrada fiscal registrada |

---

## 3. Regras de dados

### 3.1 Identificação

- `IDENTIFICACAO` = `NF com 9 dígitos` + `-` + `série sem zeros à esquerda`. Exemplo: `000027607-12`.
- É a chave usada pelas automações. **Não altere esta coluna.**

### 3.2 Canal

| Origem (`NOME_CLIFOR`) | Exibido como |
|---|---|
| CONTROLE QUALIDADE VAREJO | VAREJO |
| ESTOQUE VAREJO | VAREJO |
| CONTROLE QUALIDADE ATAC | ATACADO |
| CONTROLE QUALID FRANQUIA | FRANQUIA |
| ESTOQUE MOST MKT | MOSTRUÁRIO |
| Demais | Nome original |

### 3.3 Datas

| Coluna | Significado |
|---|---|
| `DATA_AGENDAMENTO` | Agendamento do recebimento **no CD Rio** |
| `INICIO_` / `FIM_CONFERENCIA_SUL` | Conferência da carga no SUL |
| `DATA_EMBARQUE` | Embarque do romaneio tipo `SUL X RJ (IMPORTADO)` |
| `PREVISAO_CHEGADA` | Data de embarque + 2 dias |
| `DATA_ENTRADA_FISCAL` | Digitação da entrada fiscal (notas não canceladas) |
| `DATA_RECEBIMENTO_RIO` | Data e hora da confirmação na tela |

### 3.4 Marca e referência

- NF com produtos de mais de uma marca → `MULTIMARCAS`.
- NF com mais de um produto → `MÚLTIPLAS` (regra esperada: uma NF tem um único produto, podendo dividir em cores).

### 3.5 Regras da planilha

Não é permitido: alterar o formato das colunas, apagar linhas, inserir, renomear ou mover colunas. A tela e as automações dependem da estrutura atual.

---

## 4. Arquitetura técnica

### 4.1 Workflow n8n: `Torre Importados - Producao v4`

| Rota (`/webhook/torre-importados/...`) | Método | Função |
|---|---|---|
| `app` | GET | Entrega a página HTML (pública; não contém dados) |
| `login` | POST | Valida o usuário e emite o token de sessão |
| `listar` | POST | Lê a aba `CONTROLE` e devolve as NFs |
| `salvar` | POST | Altera carro, observação ou recebimento |
| `cadastrar` | POST | Valida o lote, consulta o SQL, checa duplicidades e grava |

### 4.2 Segurança

- **Senha:** armazenada no workflow apenas como hash SHA-256 com salt (node `AUTH - Validar usuário`).
- **Token:** `usuario.expiração.assinatura`, assinado com HMAC-SHA256 e válido por 8 horas. As rotas `listar`, `salvar` e `cadastrar` recusam um token ausente, adulterado ou vencido (HTTP 401).
- **Requisições:** enviadas como formulário (`application/x-www-form-urlencoded`), sem preflight de CORS. Isso é compatível com o isolamento de HTML do n8n 2.x.
- **Entradas:** validadas no servidor (formato da NF, série, tamanho dos campos, ações permitidas). Os parâmetros do SQL contêm somente dígitos, vírgula e hífen.

### 4.3 Fontes SQL Server

`FATURAMENTO`, `FATURAMENTO_PROD`, `PRODUTOS`, `LOJAS_REDE`, `LL_RECEBIMENTO_AGENDADO`, `SS_ROMANEIOS_CARGA`, `SS_ROMANEIOS_CARGA_ITEM`, `ENTRADAS`.

### 4.4 Credenciais n8n necessárias

| Credencial | Uso |
|---|---|
| Google Sheets (service account) | Leitura e gravação da aba `CONTROLE` |
| Microsoft SQL (`sql_serve_somalog`) | Consulta de faturamento e eventos |

---

## 5. Implantação

1. Faça uma cópia de segurança da aba `CONTROLE`.
2. **Desative** a versão anterior do workflow. As rotas são as mesmas, e o n8n não aceita rotas duplicadas em workflows ativos.
3. Importe o JSON, confira as credenciais nos nodes de Google Sheets e SQL e **ative**.
4. Valide com o roteiro abaixo, usando uma NF real de cada vez.

### Roteiro de homologação

| Teste | Resultado esperado |
|---|---|
| Login com senha incorreta | "Usuário ou senha inválidos." |
| Login correto | Tabela carregada com o total de NFs |
| Trocar carro | `CARRO` e `ULTIMA_ATUALIZACAO` atualizados |
| Registrar observação | `OBSERVACAO_RECEBIMENTO` preenchida |
| Recebimento → Cancelar | Nenhuma alteração |
| Recebimento → Confirmar | `RECEBIMENTO_RIO = Sim` e `DATA_RECEBIMENTO_RIO` preenchida |
| Cadastrar 1 NF nova | Nova linha com os dados do SQL |
| Cadastrar NF já existente | Lote recusado, nenhuma linha gravada |

### Troca de senha

A senha é validada pelo hash no node `AUTH - Validar usuário`. Para trocá-la, gere um novo hash SHA-256 de `<salt>:<nova senha>`, usando o salt que está no node `AUTH - Hash senha`, e substitua `SENHA_HASH`. Nunca registre a senha em texto aberto no workflow, no repositório ou em documentos.

---

## 6. Diagnóstico de problemas

| Sintoma | Causa provável | Ação |
|---|---|---|
| Tela em branco | Acesso pela URL `/webhook-test/` ou workflow inativo | Usar `/webhook/` e conferir se o workflow está ativo |
| "Failed to fetch" / "Não foi possível conectar" | Rota não registrada (workflow inativo) ou rede | Ativar o workflow; testar `.../torre-importados/login` direto no navegador |
| "Sessão inválida ou expirada" | Token vencido (8 h) | Entrar novamente |
| NF "não localizada" | NF ou série incorreta, ou NF ainda não faturada | Revisar os dados; consultar o faturamento |
| Alteração não grava | `IDENTIFICACAO` duplicada ou alterada na planilha | Corrigir a coluna `IDENTIFICACAO` |

Para investigar, use a aba **Executions** do n8n e localize a execução da rota correspondente.

---

## 7. Riscos conhecidos e próximos passos

| Risco | Mitigação atual | Recomendação |
|---|---|---|
| Usuário compartilhado, sem rastreio individual | Data e hora em cada alteração | Integrar ao SSO corporativo |
| Senha visível nos registros de execução do login | Acesso ao n8n restrito | Reduzir a retenção de execuções; restringir administradores |
| Sem limite de tentativas de login | Senha com hash | Senha forte; avaliar bloqueio por tentativas |
| Cadastros simultâneos do mesmo lote | Checagem de duplicidade antes da gravação | Orientar um único cadastro por lote |
| Planilha como base única | Backups manuais | Backup periódico automatizado |

---

## 8. Responsabilidades

| Área | Responsável por |
|---|---|
| Faturamento SUL | Cadastrar as NFs com o carro correto |
| CD Rio | Confirmar o recebimento e registrar divergências |
| Sistema (n8n) | Validar NFs, enriquecer dados, registrar data e hora, sinalizar inconsistências |
| Sustentação | Manter o workflow, as credenciais, os backups e o controle de acesso |

---

**Confidencialidade:** não compartilhe o link da aplicação, o link da planilha ou as credenciais com pessoas de fora do time. Esta documentação não contém credenciais.
=======
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
>>>>>>> e24ef37cf6b9c17628168fcbd6e4f73fd0c69ca8
