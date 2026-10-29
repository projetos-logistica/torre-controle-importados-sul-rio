# Workflows

`01` a `04`: copias das versoes-base geradas durante o desenvolvimento. **Nao sao necessariamente os exports finais corrigidos no ambiente do usuario.** Antes de marcar uma versao Git como producao, exportar novamente cada workflow do n8n e substituir os arquivos aqui.

`05`: novo modelo de status operacional para importar e homologar. Todos os JSONs estao desativados (`active=false`), sem credenciais exportadas.

O node HTTP PATCH deve usar URL dinamica vinda de `item_id` e enviar JSON no formato apropriado ao node. Consulte `docs/deploy.md`.
