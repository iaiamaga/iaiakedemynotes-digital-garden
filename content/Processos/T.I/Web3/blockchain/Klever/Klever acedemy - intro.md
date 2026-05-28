## Introdução: configurações iniciais windows 
1. instalar powershell windows 
2. instalar ubuntu 
3. configurar conta 
4. ir para o diretório anterior ao home 
	- cd ..
	- ls 
	- cd .. 
	- ls 
5. sudo apt update 
6. sudo apt install build-essential
7.  sudo apt install curl
8.  sudo apt install net-tools
9. sudo apt install git
10. sudo apt install mc --> renomeia arquivo chamando mc
11.  curl --proto '-https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
	1. resposta: (1) Protocol "https" not supported or disabled in libcurl
	2. curl -V 
	3. resposta:
		curl 8.5.0 (x86_64-pc-linux-gnu) libcurl/8.5.0 OpenSSL/3.0.13 zlib/1.3 brotli/1.1.0 zstd/1.5.5 libidn2/2.3.7 libpsl/0.21.2 (+libidn2/2.3.7) libssh/0.10.6/openssl/zlib nghttp2/1.59.0 librtmp/2.3 
		OpenLDAP/2.6.7 Release-Date: 2023-12-06, security patched: 8.5.0-2ubuntu10.6 
		Protocols: dict file ftp ftps gopher gophers http https imap imaps ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp 
		 Features: alt-svc AsynchDNS brotli GSS-API HSTS HTTP2 HTTPS-proxy IDN IPv6 Kerberos Largefile libz NTLM PSL SPNEGO SSL threadsafe TLS-SRP UnixSockets zstd
	4. É observavel que em protocols o https aparece, portanto o curl devia sim suportar o https. Porém por algum motivo não consigo executar o comando -> curl: (1) Protocol "https" not supported or disabled in libcurl , para instalar o rust na máquina. Como resolver isso para que no final eu esteja com o rust instalado na máquina.
	5. coloquei no claude e a solução + explicação do que deu esse erro: 
		Note o **apóstrofo simples** antes de `https`: `'-https'` Isso faz com que o curl interprete como "**desabilitar** https" (o hífen `-` antes de `https` significa negação).
		Solução:
		 Use **aspas simples** ou **igual** corretamente:
	```bash
	curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
	```

	Ou simplesmente:
	```bash
	curl --proto 'https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
	```

	Alternativas se ainda houver problemas

	**Opção 1 - Sem especificar protocolo:**
	```bash
	curl --tlsv1.2 -sSf https://sh.rustup.rs | sh
	```

	**Opção 2 - Instalar via WSL (se estiver usando Ubuntu no Windows):**
	```bash
	# Baixar o script primeiro
	curl -sSf https://sh.rustup.rs -o rustup-init.sh
	
	# Executar
	sh rustup-init.sh
	```
	
	**Opção 3 - Instalar Rust nativamente no Windows:**
	
	- Baixe o instalador direto: [https://rustup.rs/](https://rustup.rs/)
	- Execute o `rustup-init.exe` no Windows (fora do WSL/Ubuntu)
	
	Após a instalação, reinicie o terminal e verifique:
	```bash
	rustc --version
	cargo --version
	```
	
	O erro era simplesmente um caractere errado no comando! 🦀
13. ``export PATHs="$HOME/.cargo/bin:$PATH"``
14. ls -la
15. source .bashrc
16. rustc --version
17. cargo --version

Viiixi, não lembro que parte eu parei aqui... mas vou dizer do que eu lembro

## Criando smart contract + build + deploy

Para fazer os contratos da Klever precisamos estar dentro do ambiente linux dentro do vscode, então para isso eu instalei a extensão wsl, e então segui os passos descritos na própria extenção: clicar em f1 e escrever wsl, vai aparecer duas opções ubuntu default e ubunto-versão, cliquei em ubunto 24. Demora um tempo, talvex precise fechar e abrir o vscode. feito isso é só entrar dnv nas extenções e procurar por klever, instala e abre a extenção. Nela vai dar para ver os tamplates. Mas antes! cria uma pasta com os comandos: 
- mkdir nome-da-sua-pasts 
e então clica na opção do vscode de abrir folder ou pasta. Depois disso no terminal digita: 
- cd nome-da-sua-pasta
só então eu clico em um template chamado "empity" e escolho o nome teste1 
ele cria uma estrutura completa pro smart contract dentro de uma pasta chamada teste1. Então agora (depois de sofrer um pouco) eu digito o seguinte comando: 
cd teste1 
rustup override set nightly-2024-09-12
importante lembrar que é preciso ter o rust instalado n ubunto não no windows (o que eu confundi alias). Se não der certo tente: 
rustup --version 
se não aparecer versão tente: 
claude. 


## O build 
o build é feito com o rust. para iniciar o build: 

Terminal: 
´´

e então clico na parta do contrato (teste1) e clico com botão direito e vou para build contract.
E aí beleza, bglh compilado... porém falta o deploy

beleza, build feito 

