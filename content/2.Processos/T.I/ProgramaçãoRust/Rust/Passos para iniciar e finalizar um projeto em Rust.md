## 

### 🚀 Iniciando um projeto

**1. Instalar o Rust (se ainda não tiver)**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

**2. Criar o projeto com Cargo**

```bash
# Binário (executável)
cargo new meu_projeto

# Biblioteca
cargo new meu_projeto --lib
```

**3. Estrutura gerada**

```
meu_projeto/
├── Cargo.toml      # Manifesto do projeto (dependências, metadados)
├── Cargo.lock      # Lock file (gerado automaticamente)
└── src/
    └── main.rs     # Ponto de entrada (ou lib.rs para libs)
```

**4. Configurar o `Cargo.toml`**

```toml
[package]
name = "meu_projeto"
version = "0.1.0"
edition = "2021"

[dependencies]
serde = { version = "1", features = ["derive"] }  # exemplo
```

**5. Adicionar dependências**

```bash
cargo add serde                # adiciona a versão mais recente
cargo add tokio --features full # com features específicas
```

**6. Desenvolver e testar no ciclo**

```bash
cargo check      # verifica erros sem compilar (rápido)
cargo build      # compila em modo debug
cargo run        # compila e executa
cargo test       # roda os testes
cargo clippy     # linter com sugestões de melhoria
cargo fmt        # formata o código
```

---

### ✅ Finalizando / Entregando um projeto

**7. Rodar todos os testes**

```bash
cargo test
cargo test -- --nocapture  # exibe prints durante os testes
```

**8. Verificar warnings e qualidade**

```bash
cargo clippy -- -D warnings   # trata warnings como erros
cargo fmt --check             # verifica formatação sem alterar
```

**9. Build de produção**

```bash
cargo build --release
# binário gerado em: target/release/meu_projeto
```

**10. Gerar documentação**

```bash
cargo doc --open   # gera e abre a doc no browser
```

**11. Publicar no crates.io (opcional)**

```bash
cargo login        # autenticar com seu token
cargo package      # empacota para verificar o que será publicado
cargo publish      # publica no crates.io
```

---

### 💡 Dicas extras

|Comando|Utilidade|
|---|---|
|`cargo update`|Atualiza dependências dentro das restrições do `Cargo.toml`|
|`cargo audit`|Verifica vulnerabilidades nas dependências|
|`cargo bench`|Roda benchmarks|
|`cargo clean`|Remove a pasta `target/`|

O fluxo essencial do dia a dia é basicamente: **`cargo check` → `cargo test` → `cargo clippy` → `cargo fmt`** antes de qualquer commit.