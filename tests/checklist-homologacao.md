# Casos de homologacao

- [ ] Faturamento cadastra NF + serie; Fluxo 01 enriquece sem duplicar.
- [ ] NF repetida em filiais diferentes nao atualiza item errado.
- [ ] Ausencia de agendamento nao gera data falsa.
- [ ] Conferencia SUL muda de inicio para fim ao longo do tempo.
- [ ] Embarque NULL nao gera previsao; se informado, D+2 corridos.
- [ ] Fluxo 02 reexecutado sem mudancas nao emite PATCH.
- [ ] Entrada fiscal aparece somente com DATA_DIGITACAO e filial validada.
- [ ] RECEBIMENTO_RIO = Sim sem data preenche horario de deteccao; rodada seguinte ignora.
- [ ] Valor textual 'Sim' e booleano true funcionam.
- [ ] Status operacional respeita prioridade entrada fiscal > recebido RIO > embarque.
- [ ] Mais de 200 linhas sao paginadas.
- [ ] Erro de API ou SQL gera alerta e nao descarta NF.
- [ ] Datas e horas conferidas com origem, considerando America/Sao_Paulo.
