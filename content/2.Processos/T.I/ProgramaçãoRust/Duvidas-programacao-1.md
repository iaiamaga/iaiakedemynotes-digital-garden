# Klever + Rust
## 1. O que é `&self`?

Sua intuição está quase certa, mas `&self` não é uma string — é uma **referência ao próprio contrato**.

Pense assim: quando você escreve um método dentro de um `trait`, o Rust precisa saber _de quem_ é esse método. O `&self` significa "esse método pertence a esta instância".

```rust
fn depositar(&self) {
    // o &self aqui dá acesso a tudo do contrato:
    self.blockchain()       // a API da blockchain
    self.saldos(&usuario)   // o storage que você definiu
    self.send()             // API de envio de tokens
}
```

O `&` na frente significa **referência** — você está "pegando emprestado" o contrato para ler/usar, sem tomar posse dele. É um conceito central do Rust para gerenciar memória com segurança.

Analogia: é como um funcionário (`self`) que carrega um crachá com acesso a todas as ferramentas da empresa. O `&` significa que ele está usando as ferramentas, não levando elas pra casa.

## 2. O que é `.blockchain()`?

É uma **API que o SDK fornece** para você consultar informações da blockchain de dentro do contrato.

Ela não guarda dados — ela **lê o estado atual** da rede no momento da execução:

```rust
self.blockchain().get_caller()           // quem chamou o contrato agora
self.blockchain().get_block_timestamp()  // timestamp do bloco atual
self.blockchain().get_sc_balance(...)    // saldo do próprio contrato
self.blockchain().get_owner_address()    // quem fez o deploy do contrato
```

Analogia: é como uma **janela para o mundo externo**. Seu contrato fica "isolado" dentro da VM, e `.blockchain()` é a janela pela qual ele espia o que está acontecendo lá fora (quem chamou, que horas são, qual o saldo, etc.).


## 3. O que são módulos?

Módulos são uma forma de **dividir o contrato em partes menores** quando ele começa a ficar grande demais.

Imagine que seu cofrinho cresceu e agora tem:

- Lógica de depósito e saque
- Lógica de recompensas
- Lógica de administração

Em vez de um arquivo `.rs` gigante com tudo misturado, você separa:

```
cofrinho/
  src/
    lib.rs          ← contrato principal, junta tudo
    depositos.rs    ← módulo de depósitos
    recompensas.rs  ← módulo de recompensas
    admin.rs        ← módulo de administração
```

Cada módulo usa `#[klever_sc::module]` em vez de `#[klever_sc::contract]`:

```rust
// Em depositos.rs
#[klever_sc::module]
pub trait Depositos {
    #[endpoint]
    fn depositar(&self) { ... }
}

// Em lib.rs — o contrato principal importa os módulos
#[klever_sc::contract]
pub trait Cofrinho: Depositos + Recompensas + Admin {
    #[init]
    fn init(&self) {}
}
```

Para o seu cofrinho atual, **você não precisa de módulos ainda** — é um recurso para quando o contrato crescer.

## 4. `require!` é tratamento de erro?

Sim, é um tipo de tratamento de erro, mas bem específico: ele **para a execução imediatamente** se a condição for falsa, e **reverte tudo** que aconteceu naquela chamada.

```rust
require!(saldo_atual >= valor, "Saldo insuficiente");
//       ^ condição que deve ser VERDADEIRA
//                              ^ mensagem de erro se for falsa
```

É equivalente a escrever isso manualmente:

```rust
if saldo_atual < valor {
    sc_panic!("Saldo insuficiente");
}
```

O ponto importante é o **revert**: se o `require!` falhar, a blockchain descarta **todas** as mudanças daquela transação, como se ela nunca tivesse acontecido. O saldo não muda, o contador não incrementa, nada.

||`require!`|`if/else` normal|
|---|---|---|
|Para a execução?|Sim, imediatamente|Só se você retornar/pausar|
|Reverte o estado?|Sim, tudo volta atrás|Não|
|Quando usar?|Validações de segurança|Lógica condicional comum|

Analogia: é como um **segurança na porta**. Se você não atende o requisito (não tem ingresso), ele te barra na entrada e você nem entra. Nada do que aconteceria lá dentro acontece.


---

# PQ os **arrays** de programação são chamados de **vetores** 
(ou arranjos) porque, na literatura de programação e na matemática, o termo descreve uma **estrutura de dados unidimensional** que armazena uma coleção de elementos do mesmo tipo, organizados sequencialmente na memória e acessíveis por um **índice numérico**. 

Enquanto o termo **array** (do inglês) é amplamente utilizado em linguagens modernas como C, Java e JavaScript, a nomenclatura **vetor** é preferida em contextos acadêmicos, em livros de lógica de programação e em algumas regiões de língua portuguesa para enfatizar a **unidimensionalidade** e a analogia com vetores matemáticos. 

- **Arranjo**: Termo técnico mais abrangente que cobre tanto estruturas unidimensionais quanto multidimensionais. 
    
- **Vetor**: Especificamente usado para **arranjos unidimensionais** (uma única linha de dados). 
    
- **Matriz**: Utilizado para **arranjos bidimensionais** ou multidimensionais.
    

A distinção de nomenclatura ajuda a identificar a complexidade da estrutura:

|   |   |   |
|---|---|---|
|Termo|Dimensão|Descrição|
|**Vetor**|1D|Uma única sequência de elementos (ex: `[1, 2, 3]`).|
|**Matriz**|2D|Uma grade de linhas e colunas (ex: `[[1, 2], [3, 4]]`).|
|**Arranjo**|Qualquer|Termo genérico para qualquer estrutura indexada.|

Em resumo, a denominação **vetor** é uma convenção histórica e didática que destaca a natureza linear e indexada da estrutura, mantendo a coerência com a terminologia matemática onde vetores são grandezas com magnitude e direção em uma dimensão específica.