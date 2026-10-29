# Microsoft Lists: Controle de Importados - SUL RIO

| Campo | Tipo recomendado | Gravador |
|---|---|---|
| Title (exibido como IDENTIFICACAO) | Texto | Faturamento |
| NOTA_FISCAL | Texto | Faturamento |
| SERIE_NF | Texto | Faturamento |
| CHAVE_NFE, MARCA, CANAL | Texto | Fluxo 01 |
| QUANTIDADE | Numero | Fluxo 01 |
| VALOR | Moeda | Fluxo 01 |
| STATUS_INTEGRACAO | Escolha ou texto | Fluxo 01 |
| DATA_AGENDAMENTO | Data/hora | Fluxo 02 |
| INICIO_CONFERENCIA_SUL, FIM_CONFERENCIA_SUL | Data/hora | Fluxo 02 |
| ROMANEIO, TRANSPORTADORA, PLACA, MOTORISTA | Texto | Fluxo 02 |
| DATA_EMBARQUE | Data/hora | Fluxo 02 |
| PREVISAO_CHEGADA | Data | Fluxo 02 (embarque + 2 dias corridos) |
| DATA_ENTRADA_FISCAL | Data | Fluxo 03 |
| RECEBIMENTO_RIO | Sim/Nao (em alguns retornos Graph: texto `Sim`) | Operacao |
| DATA_RECEBIMENTO_RIO | Data/hora | Fluxo 04 |
| STATUS_OPERACIONAL | Texto | Fluxo 05 |

A confirmacao de RECEBIMENTO_RIO e independente da conclusao fiscal. O horario do Fluxo 04 representa a primeira deteccao do `Sim` pelo n8n. Campos vazios nao devem apagar historico.
