**mapa estrutural do aprendizado**

## O MAPA DOS 20% — PROGRAMAÇÃO (COM FOCO EM RUST)



---

1.  CAMADA — LÓGICA OPERACIONAL

- decomposição de problemas
    
- sequência lógica
    
- condição (decisão)
    
- repetição (loop)
    
- controle de estado (variáveis mudando)
    

#### Por que isso vem primeiro

Porque programação não é escrever código, é:

> transformar um problema em passos determinísticos

Se essa camada não existe, tudo vira:

- tentativa e erro
    
- dependência de exemplo
    
- travamento quando o problema muda
    

---


#### Materiais úteis (gratuitos e realmente eficientes)

- Visualgo  
    → serve para ver lógica acontecendo, não para estudar teoria
    
- HackerRank (problemas básicos)  
    → usar sem foco em linguagem, só raciocínio
    

---

## 🔹 CAMADA 2 — TRADUÇÃO (LÓGICA → CÓDIGO)

- variáveis como representação de estado
    
- estruturas de controle (`if`, loops)
    
- funções como encapsulamento de lógica
    
#### Por que:

Porque código é só:

> uma linguagem para expressar lógica

Se você aprende código antes da lógica, acontece:

- você escreve sem entender
    
- depende da sintaxe
    
- não generaliza
    

> **ponte entre pensamento e execução**

Você começa a perceber que:

- o problema já estava resolvido antes do código
    
- o código só materializa a solução
    

---

### Materiais úteis

- Rust Playground  
    → ambiente sem fricção
    
- Exercism  
    → prática guiada com feedback
    

---

## 🔹 CAMADA 3 — MODELO DE EXECUÇÃO (MEMÓRIA)


- variável como espaço na memória
    
- diferença entre valor e referência
    
- tempo de vida dos dados
    
- ownership (no caso do Rust)
    

---

### Por que isso é o divisor de águas

Aqui está o ponto que separa:

- quem “usa código”
    
- de quem “entende o que o código faz”
    

Rust força isso explicitamente.

Outras linguagens escondem — por isso parecem mais fáceis no começo.

---

### Lógica didática

Essa camada constrói:

> **previsibilidade mental do comportamento do programa**

Você deixa de reagir a erros e passa a antecipar:

- “isso vai quebrar”
    
- “isso não pode existir mais”
    
- “isso foi movido”
    

---

### Materiais úteis

- The Rust Programming Language  
    → capítulos iniciais (ownership, borrowing)
    
- Rust by Example  
    → ver comportamento direto
    

---

## 🔹 CAMADA 4 — ESTRUTURAS DE DADOS ESSENCIAIS

### O que importa aprender

- listas (`Vec`)
    
- strings
    
- structs (organizar dados)
    
- enums (`Option`, `Result`)
    

---

### Por que isso vem aqui

Depois que você entende:

- lógica
    
- execução
    

agora você precisa de:

> ferramentas para modelar problemas reais

Sem isso, você só resolve problemas artificiais.

---

### Lógica didática

Essa camada constrói:

> **capacidade de modelagem**

Você passa de:

- “resolver passos”
    

para:

- “organizar informação + comportamento”
    

---

### Materiais úteis

- Rustlings  
    → prática incremental
    
- LeetCode (nível easy)  
    → aplicar estruturas em problemas
    

---

## 🔹 CAMADA 5 — PADRÕES DE RESOLUÇÃO

### O que importa aprender

- padrões recorrentes:
    
    - busca
        
    - filtragem
        
    - acumulação
        
    - transformação
        

---

### Por que isso é o “20% que vira 80%”

A maioria dos problemas reais é combinação disso.

Não são grafos complexos ou algoritmos avançados.

São variações de:

- percorrer
    
- comparar
    
- guardar resultado
    

---

### Lógica didática

Essa camada constrói:

> **reconhecimento de padrões**

Você para de ver problemas como novos e começa a ver:

- “isso é só um filtro com condição”
    
- “isso é uma variação de busca”
    

---

### Materiais úteis

- Codewars
    
- Exercism
    

---

# ⚠️ O QUE NÃO É PARTE DOS 20% (por enquanto)

Com base no curso que você mostrou:

Você pode ignorar inicialmente:

- grafos
    
- Dijkstra
    
- Floyd-Warshall
    
- Kruskal
    
- listas encadeadas complexas
    

---

## Por quê?

Porque isso exige:

- abstração avançada
    
- domínio prévio das outras camadas
    

Aprender isso cedo:

> dá sensação de progresso, mas não constrói base

---

# 🧩 CONEXÃO FINAL (o que amarra tudo)

O plano inteiro segue uma lógica:

1. **pensar (lógica)**
    
2. **expressar (código)**
    
3. **entender execução (memória)**
    
4. **modelar (estruturas)**
    
5. **reconhecer padrões (escala)**
    

Se inverter isso, você trava.  
Se seguir isso, você internaliza.

---

## 📌 Se quiser refinar mais ainda

Posso fazer um nível acima disso:

→ mapear exatamente **quais conceitos do Rust pertencem a cada camada**  
→ e quais são “ruído” no começo

Ou descer um nível:

→ pegar um problema e mostrar como ele atravessa todas as camadas

Qual direção te ajuda mais agora?