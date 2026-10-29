# Torre de Controle de Importados | SUL → CD RIO

Projeto interno para acompanhar as notas fiscais de importados desde o faturamento ate o recebimento fisico e a entrada fiscal no RIO.

**Stack:** n8n (self-hosted), SQL Server, Microsoft Lists / SharePoint, Microsoft Graph API, Git/GitHub.

## Fluxos

| Arquivo | Objetivo | Situacao |
|---|---|---|
| `workflows/01-faturamento.json` | Cadastrar NF e enriquecer chave/marca/canal/quantidade/valor | Base; trocar por export homologado |
| `workflows/02-acompanhamento-sul.json` | Agendamento, conferencia, romaneio, embarque, previsao D+2 | Base; trocar por export homologado |
| `workflows/03-entrada-fiscal.json` | Detectar ENTRADAS.DATA_DIGITACAO | Base; trocar por export homologado |
| `workflows/04-recebimento-rio.json` | Completar data para RECEBIMENTO_RIO = Sim | Base; trocar por export homologado |
| `workflows/05-status-operacional.json` | Calcular etapa operacional | Modelo novo, requer homologacao |

## Fluxo de dados

Faturamento registra `NOTA_FISCAL` e `SERIE_NF` → n8n consulta SQL Server → Microsoft Graph atualiza a mesma linha na Lista → dados SUL e RIO chegam progressivamente → status acompanha a etapa.

## Estrutura

- `workflows/`: exportacoes JSON do n8n
- `sql/`: consultas de referencia para testar no SSMS
- `docs/arquitetura.md`: fontes, relacionamento e riscos
- `docs/modelo-dados.md`: colunas da lista SharePoint
- `docs/deploy.md`: implantacao, credenciais e homologacao
- `tests/checklist-homologacao.md`: casos criticos
- `config/sharepoint.example.json`: configuracao nao secreta

## Como usar

1. Criar repositorio **privado** `torre-controle-importados-sul-rio` no GitHub.
2. Fazer commit dos arquivos deste repositorio.
3. Exportar as versoes atuais dos Fluxos 01-04 diretamente do n8n e substituir os JSONs-base.
4. Importar, configurar e homologar o Fluxo 05.
5. Conferir checklist antes de ativar.

**Aviso:** este ZIP nao contem credenciais e nao e uma copia garantida das ultimas alteracoes feitas na interface do n8n.
