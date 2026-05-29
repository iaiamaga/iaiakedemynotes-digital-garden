---
about: sobre github issue como criar no projeto protocolo social forest
created: 2026-05-22 14:21:43.4343-03:00
modified: 2026-05-22 19:08:46.4646-03:00
draft: false
tags:
  - github
---
Bom dia pessoal.
Acho relevante compartilhar com vocês uma sugestão de como poderíamos utilizar o github para melhorar nosso processo de colaborar com o código e "refinar" as coisas com menos ficção. As Issues são consideradas essenciais para manter o controle de qualidade, documentar decisões e orquestrar o trabalho da equipe em projetos, principalmente em projetos mais complexos como esse.

Dentro do repositório do projeto existe a aba " Issues", uma ferramenta nativa de rastreamento usada para gerenciar tarefas. Ele funciona como um sistema de tópicos de discussão, permitindo que mantenedores e colaboradores acompanhem o progresso de correções e melhorias, além de facilitar uma comunicação organizada sobre o projeto. 

![[Pasted image 20260522142318.png]]

Basta criar uma issue (clicando em New issue) e preencher com um título descritivo e uma descrição detalhada (em Markdown) que pode ser sobre um bug, uma nova funcionalidade, tarefas ou até mesmo ideias a serem implementadas. Algo importante é incluir passos para reproduzir bugs, logs de erro, screenshots ou contexto relevante para ajudar na resolução. 

Dá para usar etiquetas (ou Labels) para categorizar o Issue (que não necessariamente precisa ser sempre um erro) ex: "bug", "enhancement", "question" que facilita.  Também dá para atribuir (seria como delegar diretamente) o Issue a uma pessoa ou mais, definindo quem está encarregado de resolver a tarefa. 

Também dá para vincular o Issue a um Quadro (Kanban) do GitHub Projects para visualizar o fluxo de trabalho (To Do, In Progress, Done). 

Mas depois de ter adicionado o Issue e alguém ter resolvido ele, tem como fechar a issue. a boa prática seria: a pessoa que resolveu criar um  Pull Request, mas pode ser na commit msm, usando a sintaxe "#N" (onde N é o número do Issue) para vincular a mudança de código ao problema/tarefa.O mantenedor ou o criador pode fechar o Issue quando a tarefa estiver concluída. Issues fechadas permanecem no histórico e podem ser reabertas se necessário. Se usar palavras-chave como "fixes #N" ou "closes #N" no commit ou PR, o GitHub vai fechar automaticamente o Issue quando a alteração for mesclada(merge). 

Para tarefas complexas,é melhor dividir um Issue principal em subtarefas menores usando a função de subproblemas. 

Podemos manter diálogo no Issue para discutir soluções. Ou utilizar a aba de discussions
![[Pasted image 20260522144652.png]]
    

    
