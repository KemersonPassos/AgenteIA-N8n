### **Projeto: Assistente Virtual do Professor Kemerson**

Este projeto é um assistente virtual para o Professor Kemerson, chamado **Capostraste**, construído com n8n. Ele foi projetado para gerenciar diversas interações de alunos via WhatsApp. O assistente é capaz de:

* **Gerenciar banco de dados**: Cadastrar, consultar e excluir informações de alunos do banco Supabase.
* **Gerenciar agenda**: Criar, reagendar, consultar e cancelar compromissos no Google Calendar.
* **Acessar base de conhecimento**: Responder dúvidas sobre as aulas, planos e metodologia usando um documento do Google Docs.
* **Processar Múltiplos Tipos de Mídia**: Interagir com o usuário via texto, áudio e imagem.

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

1.  **Recepção e Filtragem de Mensagens**: Uma mensagem é recebida via WhatsApp e passa por um filtro para ignorar as mensagens enviadas pelo próprio bot.
2.  **Classificação por Tipo de Mensagem**: O sistema identifica se a mensagem é texto, áudio ou imagem.
    * **Áudio**: A mensagem de áudio é convertida e transcrita para texto.
    * **Imagem**: A imagem é analisada e interpretada para extrair o contexto.
    * **Texto**: A mensagem de texto segue diretamente para o próximo passo.
3.  **Acionamento do Agente Orquestrador**: O texto (original ou transcrito) é enviado ao **Agente Orquestrador**.
4.  **Verificação no Banco de Dados**: O Orquestrador **sempre** aciona o **Agente Banco de Dados** para verificar se o usuário já possui cadastro.
5.  **Identificação e Delegação de Tarefas**: Simultaneamente, o Orquestrador identifica os demais assuntos na mensagem e aciona os agentes correspondentes:
    * **Agente Agenda**: Para questões relacionadas a agendamentos.
    * **Agente Conhecimento**: Para dúvidas sobre aulas, planos e metodologia.
6.  **Compilação e Envio da Resposta**: O Orquestrador aguarda a resposta de **todas** as ferramentas acionadas, compila as informações em uma única mensagem coesa e a envia de volta ao usuário via WhatsApp.

### **Atualização da Base de Conhecimento**

O fluxo também inclui um processo automatizado para manter a base de conhecimento sempre atualizada:

1.  **Monitoramento do Google Drive**: O sistema monitora uma pasta específica no Google Drive em busca de arquivos novos ou atualizados.
2.  **Extração e Processamento de Dados**: Quando um arquivo é adicionado ou modificado, o conteúdo é extraído e processado.
3.  **Vetorização e Armazenamento**: O texto extraído é enviado para a API da Gemini para ser vetorizado e, em seguida, é armazenado no Supabase. Isso garante que o **Agente Conhecimento** tenha acesso às informações mais recentes para responder às dúvidas dos alunos.
