### **Projeto: Assistente Virtual do Professor Kemerson**

Este projeto é um assistente virtual para o Professor Kemerson, chamado **Capostraste**, construído com n8n. Ele foi projetado para gerenciar diversas interações de alunos via WhatsApp. O assistente é capaz de:

* **Gerenciar banco de dados**: Cadastrar, consultar e excluir informações de alunos do banco Supabase.
* **Gerenciar agenda**: Criar, reagendar, consultar e cancelar compromissos no Google Calendar.
* **Acessar base de conhecimento**: Responder dúvidas sobre as aulas, planos e metodologia usando um documento do Google Docs.

O assistente utiliza uma arquitetura de agentes para orquestrar as tarefas de forma eficiente.

***

### **Componentes Principais (Agentes)**

O projeto é composto por vários agentes, cada um com um papel específico:

* **Agente Orquestrador**: Se apresenta como Capostraste, o assistente virtual do professor Kemerson. Sua função é analisar a mensagem do usuário, identificar todos os assuntos e chamar as ferramentas (outros agentes) apropriadas, podendo ser mais de uma por vez. Ele não responde por conta própria, apenas compila as respostas dos outros agentes para retornar uma resposta final organizada. O agente Orquestrador sempre consulta o agente banco de dados para verificar se o usuário é um aluno cadastrado.

* **Agente Banco de Dados**: Gerencia dados de usuários em um banco de dados Supabase. Sua função é verificar o cadastro, criar novos registros, consultar informações e, se necessário, atualizar ou excluir registros.

* **Agente Agenda**: Gerencia os compromissos da agenda do professor Kemerson. Ele usa nós para Criar, Atualizar, Consultar e Cancelar eventos no Google Calendar, conforme a solicitação do usuário. Ao criar um compromisso, ele deve retornar o ID do evento ao aluno.

* **Agente Conhecimento**: Atua como a base de conhecimento sobre as aulas de violão e música do professor. Ele consulta um Google Docs para responder às perguntas dos alunos. Se a informação não estiver disponível, ele informa que não sabe a resposta.

***

### **Fluxo de Operação**

O fluxo do assistente Capostraste é orquestrado para garantir uma resposta completa e precisa:

1.  Uma mensagem é recebida e classificada por tipo (texto, áudio, imagem).
2.  O **Agente Orquestrador** é acionado.
3.  O Orquestrador **sempre** chama o **Agente Banco de Dados** para verificar o cadastro do usuário.
4.  Simultaneamente, o Orquestrador identifica outros assuntos e chama os agentes correspondentes (Agenda ou Conhecimento).
5.  O Orquestrador aguarda as respostas de **todas** as ferramentas.
6.  Por fim, o Orquestrador compila as respostas e envia uma mensagem final para o usuário.
