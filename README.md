# fluxo-de-entrevistas-n8n
📄 Fluxo de Entrevistas Automatizado - n8n Workflow
Este é um fluxo de trabalho (workflow) do n8n projetado para automatizar a triagem inicial de candidaturas para vagas de emprego. Ele gerencia todo o processo, desde a coleta de dados via formulário até a tomada de decisão automática de salário e a notificação dos próximos passos.

🚀 Funcionalidades Principais
Este fluxo realiza as seguintes etapas de forma automática:

Coleta de Dados: Recebe candidaturas através de um formulário da web nativo do n8n.

Triagem Salarial: Verifica se a Pretensão Salarial é superior a um valor limite (R$ 5.000,00 neste caso).

Se for superior, envia um e-mail de rejeição educado, informando a incompatibilidade financeira.

Se for igual ou inferior, a candidatura é aprovada para a próxima etapa.

Registro de Candidato: Salva os dados do candidato (Nome, Email, Cargo, Pretensão e Data) em uma planilha do Google Sheets.

Notificação de Aprovação: Envia um e-mail de aprovação para o candidato, indicando que ele avançou no processo.

Notificação de Gestor: Envia um e-mail para o gestor responsável (ou RH) com os detalhes do novo candidato.

Agendamento Automático (Opcional): Para candidatos com pretensão salarial abaixo de um segundo valor (R$ 4.000,00), o fluxo agenda automaticamente uma entrevista no Google Calendar e envia o link para o candidato e o gestor.

🧱 Estrutura do Workflow
Nó (Node)	Nome no Fluxo	Função
Form Trigger	Quando receber candidatura	Gatilho inicial. Cria um formulário web para receber os dados do candidato.
IF	Verifica salário	Decide o caminho a seguir: Aprovado ou Rejeitado (com base em Pretensão Salarial > R$ 5000).
Google Sheets	cadastrar usuário na planilha	Adiciona o novo candidato à planilha de controle.
Switch	Verifica cargo	Divide o fluxo em 5 caminhos, um para cada cargo, garantindo que o e-mail de aprovação seja enviado antes das próximas etapas.
Gmail	Email para dev, emial para AD, etc.	Envia a mensagem de aprovação para o candidato.
Filter	FIltro de salário	Filtra novamente os aprovados para agendamento (somente se Pretensão Salarial < R$ 4000).
Google Calendar	Agendar entrevista	Cria o evento de entrevista automaticamente.
Google Sheets	colocar link da entrevista	Atualiza a planilha de controle com o link do agendamento.
Gmail	email entrevista	Envia o e-mail de confirmação da entrevista (para candidato e gestor).
Gmail	email para gestor	Notifica o gestor sobre o novo candidato registrado.

Exportar para as Planilhas
⚙️ Pré-Requisitos e Configuração
Para utilizar este fluxo, você precisará configurar as seguintes credenciais e recursos:

Credenciais do n8n:

Gmail OAuth2: Para enviar e-mails de aprovação, rejeição e agendamento.

Google Sheets OAuth2 API: Para ler e escrever dados na planilha de candidatos.

Google Calendar OAuth2 API: Para agendar as entrevistas.

Planilha do Google Sheets:

Crie uma planilha com as colunas: Nome, Email, Cargo, Pretensão, Data Cadastro, Entrevista.

No nó cadastrar usuário na planilha, substitua o Document ID e o Sheet Name pelos dados da sua planilha.

E-mails e Regras:

No nó Salário Alto Email, personalize o e-mail de rejeição e a assinatura.

No nó Verifica salário, você pode alterar o valor de corte de 5000 para outro.

No nó FIltro de salário, você pode alterar o valor de 4000 usado para agendamento automático.

No nó email para gestor, substitua o e-mail do gestor (renatakarla663@gmail.com) pelo e-mail correto do RH/Gestor.

📥 Como Importar
Baixe o arquivo Fluxo de entrevistas.json deste repositório.

No seu painel do n8n, clique em "Workflows".

Clique em "Novo" e selecione "Importar do JSON" (ou use Ctrl/Cmd + V se tiver copiado o conteúdo do arquivo).

Configure as credenciais e planilhas conforme descrito acima.

ATIVE o fluxo de trabalho (o botão "ON") para que o formulário comece a receber candidaturas.
