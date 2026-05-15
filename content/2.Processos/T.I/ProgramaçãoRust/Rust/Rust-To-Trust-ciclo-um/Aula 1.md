

# 📦 OFICINA PRÁTICA — CICLO 1 / AULA 1

Título Técnico da Aula: O Toolchain e o Ponto de Entrada Sistêmico Nível: Profissional Domínio: Rust / Infraestrutura de Sistemas

### 1️⃣ MISSÃO OPERACIONAL

Nesta aula, estabeleceremos o alicerce técnico de um nó (node) de blockchain: o trust_node. 

O problema central que resolvemos aqui é a imprevisibilidade do ambiente de execução. Em sistemas legados, a incerteza sobre o estado inicial e a gestão de memória frouxa são vetores de falhas catastróficas. 

Os **Sistemas legado** são sistemas de software (ou hardware) antigos que ainda estão em uso em uma organização, mesmo que a tecnologia seja obsoleta ou difícil de manter. Eles continuan existindo porque substituí-los é caro, arriscado e complexo. Muitos bancos, governos e grandes empresas dependem de sistemas legado que processam milhões de transações por dia — e ninguém quer arriscar uma migração que pode dar errado. **Os desafios principais** são: segurança (vulnerabilidades sem suporte), custo de manutenção alto, dificuldade de integração com APIs e tecnologias modernas, e dependência de poucos especialistas que ainda conhecem o sistema. Um exemplo clássico são sistemas bancários escritos em COBOL nos anos 60-80 que ainda rodam hoje processando transações financeiras.

O artefato produzido será um binário nativo auditável (ou seja, uma coisa em binário de forma nativa que pode ser verificado) que servirá como o "coração" do node. Na arquitetura do bootcamp, esse artefato é o ponto zero de onde todas as validações de consenso e transações derivarão. Para sistemas Web3, isso é vital: a confiança em uma rede descentralizada não é subjetiva; ela deve ser uma propriedade emergente da prova matemática do código e da rigidez do toolchain.

Ao final desta aula, você terá produzido um artefato determinístico, auditável e pronto para evoluir no próximo ciclo.

### 2️⃣ CONTEXTO TEÓRICO FUNDAMENTAL

Rust é uma linguagem de sistemas focada em segurança, velocidade e concorrência, alcançando esses objetivos sem um coletor de lixo (garbage collector) através de um modelo de segurança de memória estrito.

Diferente de sistemas que dependem de runtimes externos ( um intermediário, tipo o JVM ), Rust compila para instruções de máquina nativas, garantindo previsibilidade desde o nível de instrução até a gestão de threads; Como não tem ninguém no meio, da para saber exatamente o que vai acontecer e quando. Nada de surpresas escondidas pelo runtime. Isso vale desde a operação mais simples (somar dois números) até coisas complexas como threads. 

#### O que é Thread
No computador, threads são "linhas de execução" que rodam ao mesmo tempo dentro do seu programa.

Imagina que seu programa é uma receita de bolo. Normalmente você segue passo a passo: mistura os ovos, depois a farinha, depois coloca no forno. **Uma coisa de cada vez.** Uma **thread** é como ter múltiplos cozinheiros na cozinha ao mesmo tempo. As coisas acontecem **em paralelo**. Exemplo: tem um programa que:
- Baixa um arquivo da internet (demora)
- Mostra uma animação de carregamento pra você
Se fosse sequencial, a animação só apareceria **depois** que o download terminasse. Com threads, as duas coisas acontecem ao mesmo tempo.

Mas podem acontecer problemas como: se vários cozinheiros na mesma cozinha tentam pegar o mesmo bowl ao mesmo tempo. No mundo dos programas isso se chama **condição de corrida** (_data race_) — duas threads tentando mexer no mesmo dado ao mesmo tempo. Isso gera bugs gravíssimos e difíceis de encontrar. Rust é famoso por **não deixar você cometer esses erros**. O compilador detecta situações perigosas e simplesmente **não compila o código** se ele puder causar problemas com threads.

Em uma blockchain, onde cada nó deve chegar exatamente ao mesmo resultado dado o mesmo input, o determinismo é a regra de ouro. Como a Blockchain é uma rede de vários computadores (nós) é preciso que todos concordem com o mesmo resultado. Se você manda a mesma operação pra 1000 computadores, todos precisam chegar na **exata mesma resposta**. Isso se chama **determinismo** — mesma entrada, sempre mesma saída, sem exceção. 

A função main em Rust não é apenas o início do código; é um contrato de engenharia onde o software assume o controle total do estado de forma segura. Em muitas linguagens, quando seu programa começa, já tem muita coisa acontecendo por baixo dos panos (o runtime inicializando, gerenciando memória, etc.) sem você saber. Em Rust, quando o `main` começa, **você está no controle de tudo**, sem surpresas escondidas. O compilador garante que esse controle seja exercido de forma segura, sem bugs clássicos de memória ou concorrência.

### 3️⃣ SETUP CONTROLADO DO AMBIENTE

A validação do toolchain é o primeiro passo de auditoria. No Linux (preferencialmente em ambiente isolado como Toolbox no Fedora Silverblue). 
Se estiver em ambiente Windows você precisa ter o WSL instalado, de preferência na máquina e a extensão do vscode. 

no terminal (linux) execute para iniciar a instalação correta:

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

source $HOME/.cargo/env

rustc --version

cargo --version

rustup update

Se estiver no windows e não sabe como abrir o terminal do ambiente linux tens 3 alternativas: 

Opção 1: Pelo menu iniciar Clica no menu iniciar do Windows, digita **"Ubuntu"** e abre o app que aparecer.

Opção 2: Pelo Windows Terminal (se tiver instalado) Abre o Windows Terminal e clica na setinha do lado do `+` para escolher **Ubuntu** na lista.

Opção 3: Pelo VS Code, abre o vscode, vai em Terminal > New Terminal, e no terminal que abrir clica na setinha do lado do `+` e escolhe Ubuntu (WSL).

Ferramentas adicionais obrigatórias.
Digite o comando no terminal:
rustup component add rustfmt clippy

- rustfmt: Garante que o código siga as diretrizes de estilo oficiais, facilitando revisões de segurança e auditorias.
    
- clippy: Um analisador estático com mais de 800 lints projetados para capturar erros lógicos e melhorar a integridade do código.
    

Em ambientes críticos, essas ferramentas não são opcionais; elas automatizam a rejeição de padrões de código inseguros antes mesmo da compilação.

Se estiver acompanhando pelo terminal recomendo que abra o VScode ou similar para continuar. Antes crie uma pasta com um nome que caracterize a aula, pelo terminal seria: 

	mkdir nome_da_pasta   
	
	ls 
	
    cd nome_da_pasta
    
Com o software instalado (VScode) basta digitar o comando:

	code .

Com a IDE aberta abra o terminal e prossiga!!
### 4️⃣ CONSTRUÇÃO DO ARTEFATO

#### 4.1 Inicialização

cargo new trust_node --bin

Utilizamos --bin para criar um executável. Na terminologia Rust, isso cria uma Crate, que é a nossa unidade fundamental de compilação.

#### 4.2 Configuração Técnica (Cargo.toml)

Confira se o arquivo Cargo.toml está dessa forma, caso não esteja edite para forçar o rigor máximo:

[package]

name = "trust_node"

version = "0.1.0"

edition = "2024" # Impõe os idiomas e padrões de segurança de 2024

  

A Edition 2024 garante que seu nó utilize as melhorias mais recentes em segurança e sintaxe, protegendo o projeto contra obsolescência técnica.

#### 4.3 Implementação Guiada

Agora iniciaremos a implementação de um programa básico em Rust, essencialmente o famoso "Hello, World!". Aqui além do código em si, está sendo introduzindo uma _filosofia_ de desenvolvimento: escrever código Rust com ênfase em **auditabilidade**, **documentação formal** e comportamento previsível. É um estilo comum em contextos de infraestrutura crítica ou sistemas que precisam ser verificáveis.

Substitua o conteúdo de src/main.rs:

/// Ponto de entrada determinístico para o nó de infraestrutura.

/// A documentação técnica é gerada via cargo doc para auditoria formal.

fn main() {

    // println! é uma macro padrão para saída de dados.

    // Macros em Rust permitem metaprogramação segura e determinística.

    println!("Node Rust for Trust: Infraestrutura validada.");

}

#### Explicação

**`fn main()`**

É a função principal, o ponto de entrada do programa. Todo executável em Rust começa por aqui. O comentário chama de "determinístico" porque Rust garante que o comportamento será sempre previsível e sem efeitos colaterais inesperados.

**`// comentários`**

As linhas começando com `//` são comentários — não são executadas, servem apenas para documentar o código. Já os comentários com `///` (três barras) são especiais: são **doc comments**, lidos pela ferramenta `cargo doc` para gerar documentação HTML automaticamente. É o padrão de documentação oficial do Rust.

**`println!("...")`**

Imprime texto no terminal. O `!` no final indica que é uma **macro**, não uma função comum. A diferença prática é que macros em Rust são expandidas em tempo de compilação, o que permite mais flexibilidade (como aceitar número variável de argumentos) mantendo segurança.

 

### 5️⃣ PRIMEIRA COMPILAÇÃO E INSPEÇÃO

Execute a compilação de desenvolvimento:

`cargo build`

#### Explicação

Pasta `target/`

Quando você compila um projeto Rust com `cargo build`, o compilador precisa guardar os resultados em algum lugar. Esse lugar é a pasta `target/`. Ela contém arquivos intermediários da compilação e o **binário final** — o executável que você vai rodar. Ela costuma ser bem pesada e por isso é comum adicioná-la ao `.gitignore`.

Debug vs Release

O Rust oferece dois modos de compilação principais:

O modo **debug** é o padrão quando você roda `cargo build`. Ele compila rápido, mas o executável gerado é mais lento, pois as otimizações são mínimas. Em compensação, ele inclui símbolos de debug, que são informações extras que ajudam a identificar erros e usar ferramentas como o debugger. O modo release é ativado com `cargo build --release`. O compilador aplica otimizações pesadas, gerando um binário significativamente mais rápido e menor. O custo é que a compilação demora mais e os símbolos de debug são removidos. Para qualquer coisa que vai para produção, esse modo é obrigatório.


Natureza do binário

Esse é um dos pontos fortes do Rust. O executável gerado é um **arquivo nativo do sistema operacional** — um `.exe` no Windows, ou um binário ELF no Linux, por exemplo. Isso significa que ele roda diretamente no hardware, sem precisar de uma máquina virtual (como a JVM do Java) ou um interpretador (como o Python). O resultado é performance próxima ao C/C++, com tempo de inicialização quase instantâneo e sem overhead de runtime externo.

- Pasta target/: Armazena os artefatos de build e o binário final.
    
- Debug vs Release: Por padrão, o build é debug (otimização mínima, símbolos de debug ativos). Para produção, o modo release é mandatório para performance.
    
- Natureza do binário: É um arquivo nativo do SO, sem dependências de VM externa, garantindo execução de alta performance.



Inspecionar e Executar:

ls -l target/debug/trust_node

./target/debug/trust_node

#### Explicação
O `ls -l` lista arquivos com detalhes. Ao apontar para o binário gerado pelo Rust, você está **inspecionando o artefato antes de rodá-lo**. Na prática, ele te mostra informações como o tamanho do arquivo, a data de geração e — mais importante — as permissões. Em Linux, um arquivo executável precisa ter a permissão `x` ativa. Você verá algo como `-rwxr-xr-x`, onde o `x` confirma que o sistema autoriza a execução. Esse hábito de inspecionar antes de executar é uma prática de engenharia responsável: você confirma que o build gerou o que era esperado.

**`./target/debug/trust_node`**

O `./` significa "execute este arquivo aqui, neste caminho". Esse comando roda o binário que o Rust compilou. Se tudo estiver correto, você verá a saída do seu `println!` no terminal. O que esse comando valida na prática é que o binário não é apenas um arquivo que compila — ele realmente funciona no ambiente real Compilar e executar são etapas distintas, e esse passo fecha o ciclo.


Juntos, os dois comandos ensinam o hábito de **verificar, depois executar** — inspecionar o artefato gerado antes de confiar nele. Em ambientes de produção e infraestrutura crítica, nunca se assume que algo funciona só porque compilou. Você confirma.
  

### 6️⃣ MICRO-EXPERIMENTO DE RIGOR

Objetivo: Demonstrar que o sistema recusa código sub-otimizado. Em resumo, esse exercício ensina sobre disciplina de engenharia, a ideia de que a qualidade não é opcional, e que ferramentas como o Clippy existem para tornar esse padrão automático e verificável.

1. Introduzir erro controlado: No main.rs, adicione: let x = 5; sem utilizá-lo.
    

2. Rodar Auditoria:  
	cargo clippy -- -D warnings   
    
3. Resultado: O Clippy falhará o build. O parâmetro -D warnings eleva qualquer aviso à categoria de erro.
    
4. Correção: Remova a variável não utilizada.
    

#### Explicação
Adiciona `let x = 5;` no código sem nunca usar a variável `x`. Para o compilador isso funciona,  o programa roda. Mas é um sinal de descuido: por que declarar algo que não serve pra nada? Pode ser um esquecimento, uma lógica incompleta, um bug escondido. Aí entra o **Clippy**, que é o linter oficial do Rust — uma ferramenta que analisa seu código em busca de problemas, más práticas e desperdícios. Ao rodar:

```bash
cargo clippy -- -D warnings
```

O parâmetro `-D warnings` faz uma coisa específica: **transforma todo aviso em erro**. Ou seja, o build falha completamente por causa de uma variável não utilizada. A correção é simplesmente remover a linha.

A cultura "Zero Warnings". Em sistemas comuns, warnings são ignorados o tempo todo — o programa funciona, então ninguém liga. Mas em sistemas críticos como contratos inteligentes e nodes de blockchain, essa tolerância é perigosa. O raciocínio é o seguinte: um warning é uma incerteza. Você não sabe exatamente por que aquilo está lá. Pode ser inofensivo, pode ser o sintoma de uma falha lógica real. Em um sistema que movimenta valor financeiro ou que precisa ser auditado por terceiros, qualquer ambiguidade é inaceitável. A cultura "Zero Warnings" trata o código como um contrato formal: se não é necessário, não existe. Se existe, precisa ser justificado.

### 7️⃣ DOCUMENTAÇÃO COMO CONTRATO

A documentação em Rust é uma especificação técnica viva. Gere-a com:

cargo doc --no-deps --open

  

- Doc comments: O uso de /// transforma comentários em documentação de API.
    
- Auditabilidade: Permite que auditores externos verifiquem a lógica do protocolo sem ler o código bruto.
    

#### Explicação 
Em muitas linguagens, a documentação é algo separado do código — um PDF, um wiki, uma página que frequentemente fica desatualizada. No Rust, a documentação vive _dentro_ do próprio código através dos `///` e é gerada automaticamente. Quando o código muda, a documentação é regerada. Elas andam juntas, por isso o termo "viva".

Ao rodar:
```bash
cargo doc --no-deps --open
```

O Rust lê todos os seus `///` e gera um site HTML navegável automaticamente. O `--no-deps` evita gerar documentação das bibliotecas externas que você usa, focando só no seu código. O `--open` já abre no navegador.

Por que isso é chamado de "contrato"? Quando você escreve `///` antes de uma função, está formalizando _o que aquela função promete fazer_. Um auditor externo — alguém que precisa verificar se o sistema é seguro e correto — pode ler essa documentação gerada sem precisar mergulhar em cada detalhe de implementação. Ele lê o contrato, não as entranhas.

Em blockchain isso é especialmente relevante porque o código frequentemente precisa ser auditado por empresas terceiras antes de ir para produção. Uma documentação bem escrita acelera e profissionaliza esse processo.
  

### 8️⃣ CHECKLIST DE VALIDAÇÃO

- [ ] Build limpo via cargo build
    
- [ ] Clippy sem warnings via cargo clippy -- -D warnings
    
- [ ] rustfmt validado (padrão de estilo OK)
    
- [ ] Edition confirmada como "2024" no Cargo.toml
    
- [ ] Binário executável manualmente em ambiente Linux
    
- [ ] Documentação gerada via cargo doc

Esse checklist representa a **porta de saída** — o conjunto mínimo de verificações que um código precisa passar antes de ser considerado pronto. Veja o que cada item significa:

**`cargo build` limpo** — o código compila sem erros. O básico do básico.

**Clippy sem warnings** — nenhum aviso, nenhuma incerteza, como discutido antes. Build com `-D warnings` garante isso de forma automática.

**rustfmt validado** — o `rustfmt` é a ferramenta oficial de formatação do Rust. Ela garante que todo código do projeto segue o mesmo estilo visual, independente de quem escreveu. Isso elimina discussões sobre indentação e torna o código mais legível para auditores.

**Edition 2024 confirmada** — o Rust lança "editions" que introduzem melhorias na linguagem sem quebrar código antigo. Confirmar a edition no `Cargo.toml` garante que você está usando as regras e recursos mais modernos da linguagem.

**Binário executável em Linux** — não basta compilar, precisa rodar. Esse item valida que o artefato final funciona no ambiente de produção real, que em infraestrutura de blockchain costuma ser Linux.

**Documentação gerada** — fecha o ciclo: o código está documentado, a documentação foi gerada e está legível.

### 9️⃣ ENTREGA TÉCNICA FINAL

Estrutura de Diretório:

trust_node/

├── Cargo.toml

├── src/

│   └── main.rs

└── target/

  

Código Final (src/main.rs):

/// Oficialização do artefato de infraestrutura do Ciclo 1.

fn main() {

    println!("Artefato determinístico trust_node pronto para evolução.");

}

  

### 🔟 MENSAGEM DE COMMIT SUGERIDA

lab: ciclo 1 aula 1 - toolchain e main definidos sob edition 2024

### CONEXÃO COM PRÓXIMO CICLO

Este artefato agora possui um ambiente de build validado e auditável. Ele permite que, na próxima aula, comecemos a preencher o estado do sistema com Primitivas de Dados e Integridade Numérica. Expandiremos a camada de segurança para lidar com overflows e precisão financeira, conceitos fundamentais para a integridade de qualquer blockchain real. Prepare-se para o rigor matemático dos tipos.

