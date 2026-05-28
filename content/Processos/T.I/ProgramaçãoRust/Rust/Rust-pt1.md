## declaração de tipos 
Rust é fortemente e estaticamente tipado, mas você não precisa escrever o tipo o tempo todo. O compilador infere com base no valor e no contexto de uso. O `char` sem anotação não é um "padrão" é inferência pura, porque aspas simples só podem ser `char`.

##### Os defaults numéricos existem, mas são específicos


Então existe sim um "tipo padrão", **mas apenas para literais numéricos** (`i32` para inteiros, `f64` para floats). Para `char`, `bool`, e outros tipos, **não existe padrão**, o compilador infere pelo valor que você atribuiu.

##### Quando você _precisa_ anotar o tipo explicitamente?

Quando a inferência não tem informação suficiente. O próprio _The Rust Book_ dá esse exemplo clássico:

rust

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Sem o `: u32`, o compilador não saberia para qual tipo numérico converter a string, e daria erro pedindo uma anotação.

---

# ARRAY

```rust
let a = [42; 100]; // array de 100 elementos, todos valendo 42
```

É equivalente a escrever `[42, 42, 42, 42, ...]` 100 vezes.
#### E o tipo do `42`?

O `42` sem nenhuma anotação é um literal inteiro — e como vimos antes, inteiros assumem `i32` por padrão. Então o compilador infere que `a` é do tipo `[i32; 100]`.

Se você quisesse outro tipo, anotaria explicitamente:

```rust
let a: [u8; 100] = [42; 100];  // array de u8
let a = [42u8; 100];            // mesma coisa, via sufixo no literal
```

---
# Tuple 
As tuplas servem para armazenar informações de diferentes tipos. 

```
fn main(

let cat = ("GATO", 4.5); //tuple

let (name, age) = cat; //forma de decompor um a //tuple para utilizar partes dela, mas geralmente é //bom usar uma struct para definir tipo

println!("chamo meu gato de {name}, ele tem {age} e come muito bem.");

)

```