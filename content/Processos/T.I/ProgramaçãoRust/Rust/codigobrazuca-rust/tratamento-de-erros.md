- Error Handling 
- Tipos de erro
- Option
- Match x Option 
- Result 
- Operador ?
- Convertendo result para option e virce versa 
- Encademaneto de map() and then() 
- Display para erro 
- Parte prática

# inicio
Rust trata os erros de forma segura, não usa exeções tradicionais. Em vez disso o desenvolvedor precisa lidar explicitamente com os erros. Isso torna o código mais seguro, previsível e confiável. 

## Tipos de erro
Em rust existem alguns tipos de erro, 

o panic! é usado quando uma parte do código não deve falhar no código normal. 

O option é para informar se existe a presença ou ausência de algo, ele garante que acessamos um valor apenas quando ele está presente, evitando erro ao tentar usar um valor ausente, exemplo: 
``` 
fn divide(a: i32, b: i32) -> Option<i32> {
 if b == 0 { None } else { Some(a / b)}
}
```
O desenvolvedor deve prever essa possibilidade e adicionar esse possível caso. Para retornar o resultado do erro o Rust obriga a considerar os dois casos com match: 
```

match buscar_usuario(1) {
    Some(nome) => println!("Usuário encontrado: {}", nome),
    None       => println!("Nenhum usuário com esse ID"),
}

``` 
💡 Rust não deixa você ignorar o caso `None`. Se tentar usar o valor direto sem verificar, **o código nem compila**. Isso evita bugs silenciosos.



O Result deve ser usado para ver se deu certo ou errado dizendo o motivo, ou seja ele tem uma descrição. exemplo: 
```
fn dividir(a: i32,b: i32) -> Result<i32, String> {
	if b == 0 { Err(String::from("Não é possível dividir por zero")) } 
	else { Ok(a / b) } 

}

// Tratando com match:

match dividir(10, 2) {
    Ok(resultado) => println!("Resultado: {}", resultado),
    Err(motivo)   => println!("Erro: {}", motivo),
}

```


O Operador ? simplifica a propagação de erros que nada mais é do que retornar automaticamente um erro quando ocorre uma falha sem a necessidade de usar o match. é usado, por exemplo, quando há etapas encadeadas e qualquer uma pode falhar; exemplo: 

```
fn carregar_config() -> Result<String, std::io::Error> {

	let conteudo = std::fs::read_to_string("config.toml")?;
	Ok(conteudo)

}
 
```

O `?` diz: _"se deu erro aqui, interrompe tudo e devolve esse erro automaticamente"_. 


Há uma forma de converter Result para Option: o .ok() ele descarta o erro e retorna None caso a aoperação falhe.Isso é util quando o motivo do erro não é relevante exemplo:

``` 
let numero: Option<i32> = Some(10);

let resultado: Result<i32, &str> = numero.ok_or("Nenhum resultado encontrado");

```

| Ferramenta | O que faz                            | Quando usar                                 |
| ---------- | ------------------------------------ | ------------------------------------------- |
| `Option`   | Tem ou não tem                       | Ausência é algo normal e esperado           |
| `Result`   | Deu certo ou deu errado (com motivo) | Falha é possível mas recuperável            |
| `?`        | Propaga o erro pra cima              | Quando quem chamou deve decidir o que fazer |
| `panic!`   | Mata o programa                      | Erro impossível de recuperar, bug grave     |

--- 

### Encadeamento com map() e then()


[^1]: 
