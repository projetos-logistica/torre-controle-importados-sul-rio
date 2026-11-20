# README - Controle de NFs Importados SUL x CD Rio
📦 Visão Geral

Com o objetivo de simplificar o acompanhamento das transferências de produtos importados entre o SUL e o CD Rio, o processo deixa de ser controlado pela lista do SharePoint e passa a ser realizado por meio de uma planilha Google integrada a automações.

## A nova solução permite:

Cadastro rápido das Notas Fiscais;
Confirmação do recebimento no CD Rio;
Atualização automática das informações logísticas;
Rastreabilidade ponta a ponta da movimentação;
Redução de controles manuais;
Maior confiabilidade das informações.

⚠️ A lista antiga do SharePoint permanecerá disponível apenas para consulta histórica.

🔄 Fluxo do Processo
Plain Text
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
Confirmação da Entrada
1️⃣ Cadastro da Nota Fiscal (Faturamento)
Objetivo

Registrar a Nota Fiscal no momento da expedição para iniciar o monitoramento automático.

Link de Cadastro

🔗 https://n8n.somalabs.com.br/form/a1272b6f-3ffa-40bb-b713-6707dec59836

# Como preencher
Opção 1

Informar:

Número da NF
Série da NF
Opção 2

Informar:

Chave de acesso da NF

💡 A chave pode ser capturada automaticamente utilizando um leitor de código de barras diretamente no DANFE.

Informações obrigatórias

Além dos dados da NF, informar:

Número do carro / veículo de transporte
Validações automáticas

Ao informar os dados, o sistema realiza a validação em tempo real e apresenta:

Referência
Descrição do produto
Marca
Canal
Quantidade
Valor da NF
Possíveis mensagens de erro
Situação	Ação
Nota não localizada	Revisar NF/Série ou informar a chave
Nota já cadastrada	Verificar cadastro existente
Nota localizada em mais de uma filial	Utilizar a chave de acesso
2️⃣ Confirmação de Recebimento (CD Rio)
Objetivo

# Registrar oficialmente o recebimento físico da carga.

Link de Recebimento

🔗 https://n8n.somalabs.com.br/form/9c38dd32-8273-4c09-93b9-483d265f0540

Como preencher

Informar:

Número da NF
Série

ou

Chave de acesso
Registro de divergências

Qualquer diferença identificada durante o recebimento deverá ser registrada no campo Observação.

Exemplos
Plain Text
FALTA 1 M
 
SOBRA 2 P
 
FALTA 3 G / SOBRA 1 M
Registro automático

No momento da confirmação, o sistema grava automaticamente:

Data do recebimento
Horário do recebimento

Não é necessário preencher essas informações manualmente.

3️⃣ Acompanhamento Operacional
Planilha de Controle

🔗 https://docs.google.com/spreadsheets/d/1IR2x958ruchBiYUOOAECPMXTxhrPR5Js2KaVuTJbFSo/edit?usp=sharing

Atualizações automáticas

A planilha é atualizada automaticamente:

Frequência	Horário
Segunda a sábado	Das 07h00 às 19h00
Intervalo	A cada 1 hora
Informações atualizadas pelo sistema

As seguintes informações são preenchidas automaticamente:

Agendamento
Conferência no SUL
Romaneio
Transportadora
Placa
Motorista
Data de embarque
Previsão de chegada
Entrada Fiscal
Edição de campos
🟩 Cabeçalho Verde

Campos destinados ao preenchimento manual pelos usuários.

⬜ Cabeçalho Cinza

Campos preenchidos automaticamente pelo sistema.

Esses campos são:

Protegidos
Não editáveis
Tratamento de inconsistências

Caso exista qualquer problema relacionado à NF, o detalhamento será exibido automaticamente na coluna:

MENSAGEM

Exemplos:

Nota não localizada
Nota duplicada
Divergência cadastral
Falha de integração
⚠️ Regras Importantes
NÃO altere a coluna IDENTIFICACAO

Esta coluna é utilizada pelas automações para identificar cada registro.

Alterações podem causar:

Falhas de atualização;
Perda de rastreabilidade;
Erros de processamento.
NÃO altere a estrutura da planilha

Não é permitido:

❌ Alterar formato das colunas

❌ Apagar linhas

❌ Inserir novas colunas

❌ Renomear colunas

❌ Mover colunas de posição

Segurança de Acesso

Os acessos aos formulários e à planilha são protegidos por autenticação.

Credenciais

Usuário e senha serão enviados separadamente.

Atenção

Não compartilhe:

Links dos formulários
Link da planilha
Credenciais de acesso

com pessoas externas ao time.

📋 Responsabilidades
Faturamento SUL

Responsável por:

Cadastrar a NF
Informar corretamente os dados do transporte
Garantir que a NF esteja disponível para acompanhamento
CD Rio

Responsável por:

Confirmar o recebimento
Registrar divergências
Garantir a rastreabilidade da entrega
Sistema

Responsável por:

Atualizar dados logísticos automaticamente
Validar informações das NFs
Registrar data e horário das movimentações
Sinalizar inconsistências
✅ Benefícios da Nova Solução
Eliminação da manutenção manual na lista SharePoint
Processo mais simples e rápido
Redução de erros operacionais
Atualização automática das informações logísticas
Maior rastreabilidade das transferências
Consulta centralizada em um único ambiente
Histórico completo das movimentações
Melhor visibilidade para operação, faturamento e recebimento
🚀 Resultado Esperado

Garantir o acompanhamento integral das Notas Fiscais de importados entre o SUL e o CD Rio, com informações atualizadas automaticamente, rastreabilidade completa e redução significativa de controles manuais e retrabalho operacional.