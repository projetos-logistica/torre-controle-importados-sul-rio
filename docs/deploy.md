# Publicacao e implantacao

## Antes de importar
- Clone este repositorio ou baixe o ZIP.
- Confirme as colunas e seus nomes internos via Microsoft Graph.
- No n8n configure credencial OAuth2 do Graph e credencial Microsoft SQL; nao armazene secrets no JSON.
- Exporte os fluxos finais diretamente do seu n8n e substitua os arquivos correspondentes em `workflows/`. **Os arquivos 01 a 04 aqui sao bases anteriores a correcoes manuais da conversa e NAO devem substituir as versoes homologadas sem revisao.**
- Teste as URLs PATCH como strings sem quebras de linha, o Body com `Using Fields Below`, e as comparacoes.
- Importe o Fluxo 05 como novo workflow, selecione a credencial e execute manualmente.
- Ative so depois de validar leitura completa, paginacao, duplicidades e fuso horario.

## Operacao
Agende 01-05 com distribuicao de minutos para evitar picos de API. No historico Executions, confira falhas. Na importacao, credenciais nao sao transportadas automaticamente.

## Seguranca
Escolha repo PRIVATE. Site ID/List ID sao metadados, mas tambem merecem uso interno. Nunca comitar Client Secret, tokens, password SQL ou export de credenciais.
