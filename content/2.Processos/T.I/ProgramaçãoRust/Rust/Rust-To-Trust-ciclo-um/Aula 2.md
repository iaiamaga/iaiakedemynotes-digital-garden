# 📦 OFICINA PRÁTICA — CICLO 1 / AULA 2-vf

Título Técnico da Aula: Primitivas de Dados e Integridade Numérica Nível: Profissional Domínio: Rust / Engenharia de Sistemas / Blockchain

### 1️⃣ MISSÃO OPERACIONAL

Nesta aula, implementaremos a lógica de representação de valores e controle de suprimento no trust_node. O problema central que resolvemos é a instabilidade aritmética. Em sistemas legados, falhas de overflow/underflow silenciosas são vetores primários para ataques de inflação e corrupção de estado.

O artefato produzido será um módulo de cálculo de saldo com verificação de limites. Na arquitetura do node, este componente garante que cada mutação de estado numérico seja validada antes da persistência. Para sistemas Web3, onde o valor é a informação, a integridade numérica é a base da confiança econômica.

Ao final desta aula, você terá produzido um artefato determinístico, auditável e pronto para evoluir no próximo ciclo.

### 2️⃣ CONTEXTO TEÓRICO FUNDAMENTAL

A arquitetura de Rust oferece uma vasta gama de inteiros (u8 a u128, i8 a i128) para modelagem de precisão. Em sistemas financeiros e blockchain, a soma direta com o operador + é considerada uma prática inaceitável.

Determinismo e Comportamento de Overflow:

- Debug Mode: O Rust gera um panic automático ao detectar overflow, interrompendo a execução para evitar dados corrompidos.
    
- Release Mode: Por padrão, o Rust realiza o wrapping modular (ex: 255u8 + 1 resulta em 0). Em um ledger, isso permitiria que um usuário "resetasse" seu saldo para zero ou gerasse valores espúrios silenciosamente.
    

APIs de Integridade Numérica:

1. checked_add: Retorna um Option. É a escolha correta para o Ledger porque força o desenvolvedor a tratar o erro caso o limite seja atingido.
    
2. saturating_add: Estagna no valor máximo (MAX) ou mínimo (MIN), útil para métricas, mas perigoso para saldos.
    
3. overflowing_add: Retorna o resultado e um booleano indicando se houve overflow.
    

Para sistemas blockchain, o uso de APIs checked_* é mandatório para garantir que a transição de estado seja válida ou explicitamente rejeitada.

### 3️⃣ SETUP CONTROLADO DO AMBIENTE

Assumimos o ambiente validado na Aula 1. Confirme as versões para conformidade com a Edition 2024:

rustc --version # Esperado: 1.90.0 ou superior

cargo --version

rustup update

  

As ferramentas clippy e rustfmt devem estar instaladas e integradas ao seu workflow.

### 4️⃣ CONSTRUÇÃO DO ARTEFATO

#### 4.1 Inicialização

Navegue até o projeto trust_node criado anteriormente.

#### 4.2 Configuração Técnica (Cargo.toml)

Verifique se a edição está correta:

[package]

name = "trust_node"

version = "0.1.0"

edition = "2024"

  

#### 4.3 Implementação Guiada

Substitua o conteúdo de src/main.rs. Implementaremos uma simulação de depósito que utiliza integridade numérica estrita:

/// Módulo de integridade numérica do Node.

/// Representa a lógica de Ledger com proteção contra instabilidade aritmética.

fn main() {

    // Definição de saldo inicial usando u64 (padrão para saldos em blockchain)

    let current_balance: u64 = 1_000_000;

    let deposit_amount: u64 = 500_000;

  

    // Decisão de Engenharia: Substituição do operador '+' por 'checked_add'.

    // O uso de .expect() garante que o sistema interrompa (panic)

    // com uma mensagem clara se a integridade do Ledger for violada.

    let new_balance = current_balance

        .checked_add(deposit_amount)

        .expect("ERRO CRÍTICO: Overflow detectado no Ledger do Node");

  

    println!("Novo estado de saldo validado: {} unidades", new_balance);

}

  

### 5️⃣ PRIMEIRA COMPILAÇÃO E INSPEÇÃO

cargo build

  

O compilador agora garante que não há conversões implícitas perigosas. O binário gerado em target/debug/trust_node carrega as verificações de runtime necessárias para o modo de desenvolvimento.

### 6️⃣ MICRO-EXPERIMENTO DE RIGOR

Objetivo: Validar a segurança de tipos e o uso de conversões semânticas.

Introduzir erro de tipos: No main.rs, tente somar um u32 a um u64:  
let a: u32 = 100;

let b: u64 = 200;

let c = b.checked_add(a).expect("Erro"); // Falha de compilação: tipos incompatíveis

1.   
    

Executar Auditoria:  
cargo clippy -- -D warnings

2.   
    

Correção Profissional: Em vez de usar as (que pode truncar valores silenciosamente), use a Trait From:  
let c = b.checked_add(u64::from(a)).expect("Overflow");

3.   
    

Racional: u64::from(a) é semanticamente seguro e garantido pelo sistema de tipos para não perder dados, ao contrário de as, que é um cast de força bruta.

### 7️⃣ DOCUMENTAÇÃO COMO CONTRATO

Atualize e visualize a especificação formal:

cargo doc --no-deps --open

  

Certifique-se de que a lógica de tratamento de overflow via expect está clara na documentação técnica.

### 8️⃣ CHECKLIST DE VALIDAÇÃO

- [ ] Aritmética Segura: Nenhum operador + ou - direto em variáveis de saldo.
    
- [ ] APIs checked: Uso obrigatório de checked_add com tratamento de erro.
    
- [ ] Sem Casting Perigoso: Uso de u64::from() para conversões seguras.
    
- [ ] Edition 2024: Confirmada no manifesto.
    
- [ ] Clippy: Zero warnings com -D warnings.
    
- [ ] Execução: Binário executado e validando o estado numérico.
    

### 9️⃣ ENTREGA TÉCNICA FINAL

Código Final (src/main.rs):

/// Oficialização do Ledger Experimental - Ciclo 1 Aula 2.

/// Implementa integridade numérica para representação de valor.

fn main() {

    let ledger_supply: u64 = 10_000_000;

    let transaction_fee: u64 = 50;

  

    // Aritmética determinística e segura

    let final_state = ledger_supply

        .checked_add(transaction_fee)

        .expect("Falha na integridade do Ledger: Limite de suprimento atingido");

  

    println!("Estado Final Auditado: {} unidades", final_state);

}

  

### 🔟 MENSAGEM DE COMMIT SUGERIDA

lab: ciclo 1 aula 2 - integridade numérica com checked_add e safe conversion

### 11️⃣ CONEXÃO COM PRÓXIMO CICLO

Nosso nó agora possui "consciência numérica" e proteção contra falhas de memória aritmética. Contudo, esses dados ainda estão soltos na função main. Na Aula 3, elevaremos a abstração através da Modelagem de Domínio com Tipos Customizados. Utilizaremos structs e enums para encapsular o estado do nó, criando a estrutura formal de um Bloco e de uma Transação. Introduziremos o conceito de Estado Encapsulado, preparando o terreno para a imutabilidade sistêmica e validação via pattern matching.
