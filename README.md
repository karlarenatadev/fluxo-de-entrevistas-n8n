# 📄 Fluxo de Entrevistas Automatizado com n8n

[![Licença: MIT](https://img.shields.io/badge/Licen%C3%A7a-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Este repositório contém um workflow completo para a plataforma de automação **n8n**, projetado para automatizar e otimizar o processo de triagem inicial de candidatos a vagas de emprego. O fluxo gerencia de forma inteligente todo o ciclo, desde a captura de dados de um formulário web até a tomada de decisões com base em critérios salariais, registro em planilhas, agendamento de entrevistas e comunicação com candidatos e gestores.

## 🚀 Funcionalidades Principais

Este workflow automatiza as seguintes etapas do processo seletivo:

-   **Coleta de Dados Centralizada**: Recebe e organiza as candidaturas através de um formulário web customizável, gerado diretamente no n8n.
-   **Triagem Salarial Inteligente**: Avalia a pretensão salarial do candidato em relação a um teto pré-definido (neste fluxo, R$ 5.000,00).
    -   **Rejeição Automática**: Se a pretensão for superior ao limite, um e-mail formal e educado é enviado ao candidato, informando a incompatibilidade financeira e mantendo uma boa experiência do candidato.
    -   **Aprovação para Próxima Etapa**: Se a pretensão for compatível, o fluxo prossegue para as próximas fases.
-   **Registro Automatizado de Candidatos**: Armazena os dados dos candidatos aprovados (Nome, Email, Cargo, Pretensão Salarial e Data) em uma planilha do Google Sheets, criando um banco de talentos organizado.
-   **Comunicação Padronizada**:
    -   **Notificação de Aprovação**: Envia um e-mail para o candidato, confirmando que ele avançou no processo seletivo.
    -   **Alerta para o Gestor**: Notifica o gestor ou o RH sobre a nova candidatura, enviando os detalhes do candidato para análise.
-   **Agendamento Automático de Entrevistas**: Para candidatos com pretensão salarial abaixo de um segundo valor de corte (R$ 4.000,00), o workflow:
    -   Cria um evento no Google Calendar.
    -   Convida o candidato e o gestor para a entrevista.
    -   Atualiza a planilha do Google Sheets com o link da videochamada.
    -   Envia um e-mail de confirmação do agendamento para todas as partes envolvidas.

## 🧱 Estrutura do Workflow

O fluxo de trabalho é composto pelos seguintes nós (nodes), orquestrados para executar o processo de forma lógica e eficiente:

| Nó (Node)              | Nome no Fluxo                  | Função                                                                                                                    |
| ---------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Form Trigger** | `Quando receber candidatura`   | Cria um formulário web para receber os dados dos candidatos (Nome, Telefone, Email, Função, Pretensão Salarial).            |
| **IF** | `Verifica salário`             | Bifurca o fluxo com base na pretensão salarial (maior que R$ 5.000,00).                                                    |
| **Gmail** | `Salário Alto Email`           | Envia o e-mail de rejeição para candidatos com pretensão salarial acima do teto.                                          |
| **Google Sheets** | `cadastrar usuário na planilha`| Adiciona os dados do candidato aprovado a uma nova linha na planilha de controle.                                         |
| **Switch** | `Verifica cargo`               | Direciona o fluxo com base no cargo selecionado pelo candidato (Programador, Analista de Dados, etc.).                      |
| **Gmail** | `Email para dev`, `emial para AD`, etc. | Envia um e-mail de aprovação personalizado, informando que o candidato avançou para a próxima fase.                |
| **Filter** | `FIltro de salário`            | Filtra os candidatos aprovados cuja pretensão salarial é inferior a R$ 4.000,00 para o agendamento automático.             |
| **Google Calendar** | `Agendar entrevista`           | Agenda a entrevista no Google Calendar, convidando o candidato e o gestor.                                                |
| **Google Sheets** | `colocar link da entrevista`   | Atualiza a linha do candidato na planilha com o link da entrevista agendada.                                              |
| **Gmail** | `email entrevista`             | Envia um e-mail de confirmação do agendamento com o link da videochamada para o candidato e o gestor.                       |
| **Gmail** | `email para gestor`            | Notifica o gestor sobre o novo candidato registrado na planilha para que ele possa dar continuidade ao processo.           |

## ⚙️ Pré-requisitos e Configuração

Para implementar este workflow em seu ambiente n8n, você precisará das seguintes credenciais e configurações:

### Credenciais do n8n

Certifique-se de que suas credenciais OAuth2 para os seguintes serviços do Google estão configuradas em sua instância do n8n:
-   **Gmail**: Para enviar os e-mails de aprovação, rejeição e agendamento.
-   **Google Sheets**: Para ler e escrever dados na planilha de candidaturas.
-   **Google Calendar**: Para criar os eventos de entrevista.

### Planilha do Google Sheets

1.  Crie uma nova planilha no Google Sheets com as seguintes colunas:
    -   `Nome`
    -   `Email`
    -   `Cargo`
    -   `Pretensão`
    -   `Data Cadastro`
    -   `Entrevista`
2.  No nó **`cadastrar usuário na planilha`**, atualize os campos `Document ID` e `Sheet Name` com os dados correspondentes da sua planilha.

### Personalização do Fluxo

-   **Valores de Triagem**:
    -   No nó **`Verifica salário`**, ajuste o valor de corte (`5000`) conforme a política da sua empresa.
    -   No nó **`FIltro de salário`**, altere o valor (`4000`) para definir o teto do agendamento automático de entrevistas.
-   **Templates de E-mail**:
    -   Personalize o conteúdo dos e-mails nos nós `Salário Alto Email`, `Email para dev`, `email entrevista`, etc., para adequá-los ao tom de voz da sua marca.
-   **Destinatário do Gestor**:
    -   No nó **`email para gestor`**, substitua o endereço `renatakarla663@gmail.com` pelo e-mail do responsável pelo processo seletivo (RH ou gestor da vaga).

## 📥 Como Importar e Ativar o Workflow

1.  Faça o download do arquivo `Fluxo de entrevistas.json` deste repositório.
2.  Acesse o seu painel do n8n e vá para a seção "Workflows".
3.  Clique em **"New"** e selecione a opção **"Import from JSON"**.
4.  Cole o conteúdo do arquivo JSON na área indicada ou faça o upload do arquivo.
5.  Siga os passos da seção de **Configuração** acima para ajustar o workflow às suas necessidades.
6.  **ATIVE** o fluxo de trabalho no botão de toggle no canto superior esquerdo para que o formulário web comece a receber candidaturas.

## 🤝 Contribuições

Contribuições são sempre bem-vindas! Se você tiver sugestões para melhorar este fluxo de trabalho, sinta-se à vontade para abrir uma *issue* ou enviar um *pull request*.

## 📜 Licença

Este projeto é distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
