# Arquitetura

Faturamento cadastra NF e serie em Microsoft Lists; Fluxo 01 consulta SQL Server e enriquece. Fluxo 02 consulta eventos SUL; Fluxo 03 consulta entrada fiscal; Fluxo 04 observa seletor RECEBIMENTO_RIO e registra horario de deteccao; Fluxo 05 calcula STATUS_OPERACIONAL. Cada fluxo atualiza somente suas colunas via Microsoft Graph PATCH /items/{id}/fields.

## Fontes SQL

- FATURAMENTO + FATURAMENTO_PROD + PRODUTOS + LOJAS_REDE
- LL_RECEBIMENTO_AGENDADO
- SS_ROMANEIOS_CARGA_ITEM + SS_ROMANEIOS_CARGA
- ENTRADAS

## Pontos para endurecimento

1. Paginar respostas Microsoft Graph com `@odata.nextLink`; `$top=200` nao basta em producao.
2. Verificar se a entrada fiscal pertence ao CD RIO antes de marcar conclusao.
3. Padronizar fuso do SQL Server/SharePoint: timestamps SQL sem timezone podem ser serializados incorretamente como UTC.
4. Selecionar romaneio/agendamento vigentes quando houver varios registros.
5. Evitar sobrescritas, implementando comparacao e observabilidade.
6. Se a confirmacao fisica precisar do horario exato, usar evento e nao a leitura periodica; o Fluxo 04 registra a deteccao.
7. Tratar notas duplicadas entre filiais, consultas sem retorno e falhas de rede.
8. Credenciais OAuth/SQL configuradas exclusivamente no n8n, nunca no Git.
