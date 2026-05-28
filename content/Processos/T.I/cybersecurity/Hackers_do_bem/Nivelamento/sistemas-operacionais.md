# Tipos de usuários 
## comandos de texto 

Comandos de texto
Comandos Linux - Manipulação de
Arquivos
• ls (list - listar)
• clear (limpar)
• cd (change directory - mudar diretório)
• mkdir (make directory - cria diretório)
• nano (editor de texto)
• mv (move - mover ou renomear)
• cp (copy - copiar)
• rm (remove - remover)

Comandos Linux - Manipulação de
Arquivos 2
• pwd (print work directory) -
imprime caminho
• cat (concatenar) ou imprime
arquivo
• sudo (super user do) - elevação
de privilégios
• ip a (ip address) - exibe
endereços
• top / btop (gerenciador de
tarefas)
• df -h
• grep

## usuários 
Usuários
Pesquise por usuário.
Acesse as configurações de usuário.
Faça o desbloqueio para permitir a
criação de novos usuários.
Adicione um novo usuário.

aqui os usuários criados deverão ter senha forte. 

----
Pelo terminal é possível utilizar o comando
useradd ou adduser.

Para conferir acesse o /etc/passwd e verifique
se o usuário foi adicionado a lista.

/etc/passwd > usuários/grupos e diretórios.

/etc/shadows > senhas criptografadas dos usuários.

Nome do usuário:! bloqueado* não pode logar.

Para alterar uma senha de um usuário,
o comando sudo passwd “nome”
permite atualizar ou criar a senha.
O comando adduser é um script que
facilita o processo de criação de
usuário, já pedindo senha, criando
grupos e configurando pasta home,
mas não são todas distribuições que
possuem esse comando

usermod -> muda o usuario de grupo
nano /etc/sudoers
nano /etc/gshadow