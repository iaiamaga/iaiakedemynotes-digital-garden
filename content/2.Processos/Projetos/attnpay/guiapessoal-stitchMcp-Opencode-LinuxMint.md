# Guia Pessoal — Stitch MCP + OpenCode no Linux

_A aventura completa, com os erros e as soluções reais_

---

## Contexto

Esse guia documenta o processo real de conectar o Google Stitch ao OpenCode CLI via MCP num Linux x86_64 (Ubuntu). Inclui os erros que aconteceram no caminho e como foram resolvidos — porque na prática as coisas não funcionam na primeira tentativa.

**Objetivo:** fazer o OpenCode conseguir acessar o design do Stitch em tempo real via MCP, sem precisar copiar e colar HTML manualmente.

---

## O que é cada peça

|Ferramenta|Para que serve|
|---|---|
|**Google Stitch**|Gera interfaces e design systems a partir de prompts|
|**stitch-mcp**|Proxy local que conecta o Stitch ao agente de código via MCP|
|**Google Cloud CLI (gcloud)**|Só para autenticação com a conta Google — não gera custos|
|**OpenCode CLI**|Agente de IA que lê o design e gera/edita o código|

---

## Passo 1 — Instalar o Google Cloud CLI

> Referência: [docs.cloud.google.com/sdk/docs/install-sdk](https://docs.cloud.google.com/sdk/docs/install-sdk?hl=pt-br)

**Como saber sua arquitetura antes de baixar:**

```bash
uname -m
# x86_64 = Linux 64 bits (mais comum)
# aarch64 = Linux Arm
```

**Instalação para x86_64:**

```bash
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

Durante a instalação: responda `Y` para as duas perguntas (adiciona o gcloud ao terminal automaticamente).

**Feche e reabra o terminal**, depois verifique:

```bash
gcloud --version
```

> **O que é o rc file?** Durante a instalação ele pergunta qual arquivo de configuração do terminal atualizar. Pode apertar Enter para aceitar o padrão `/home/seuusuario/.bashrc`.

---

## Passo 2 — Autenticar no Google

Esses comandos valem para todo o computador — podem ser rodados de qualquer pasta:

```bash
gcloud auth login
```

Abre o navegador. Faça login com sua conta Google e autorize.

```bash
gcloud auth application-default login
```

Abre o navegador de novo. Autorize novamente — isso permite que o MCP use suas credenciais automaticamente.

> O aviso `WARNING: Cannot find a quota project` pode ser ignorado. Não afeta o uso gratuito.

---

## Passo 3 — Testar o servidor MCP do Stitch

> Referência: [github.com/davideast/stitch-mcp](https://github.com/davideast/stitch-mcp)

```bash
STITCH_API_KEY=suachaverealaaqui npx @_davideast/stitch-mcp proxy
```

**Resultado esperado:**

```
[stitch-proxy] Connected to Stitch, discovered 14 tools
[stitch-proxy] Proxy server running
```

Se aparecer isso, funcionou. Aperte `Ctrl+C` para parar — era só um teste.

> **Onde pegar a API key?** No painel do Stitch, nas configurações do projeto.

---

## Passo 4 — Deixar a API key permanente no terminal

Para não precisar exportar manualmente toda vez que abrir um terminal:

```bash
code ~/.bashrc
```

Adicione no final do arquivo:

```bash
export STITCH_API_KEY=suachaverealaaqui
```

Salve e aplique:

```bash
source ~/.bashrc
```

---

## Passo 5 — Configurar o OpenCode no projeto

Navegue até a pasta do projeto:

```bash
cd caminho/do/seu/projeto
```

Crie o `.env` para guardar variáveis do projeto:

```bash
code .env
```

```
STITCH_API_KEY=suachaverealaaqui
```

Crie o `opencode.json`:

```bash
code opencode.json
```

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["AGENTS.md", "DESIGN.md", "PRD.md"],
  "permission": {
    "edit": "ask",
    "bash": "ask"
  },
  "mcp": {
    "stitch": {
      "type": "local",
      "command": ["npx", "@_davideast/stitch-mcp", "proxy"],
      "enabled": true,
      "environment": {
        "STITCH_API_KEY": "{env:STITCH_API_KEY}"
      }
    }
  }
}
```

> **Atenção ao formato:** `command` deve ser um array. `environment` (não `env`) é a chave correta para o OpenCode. O `{env:STITCH_API_KEY}` lê a variável do sistema automaticamente.

---

## Passo 6 — Criar o AGENTS.md com as regras do agente

```bash
code AGENTS.md
```

```markdown
# Regras do Agente — [Nome do Projeto]

## MCP e Design
- SEMPRE use o Stitch MCP para buscar informações visuais antes
  de implementar qualquer tela ou componente
- Use a ferramenta `stitch` para inspecionar o design antes de escrever código
- Se o MCP não estiver acessível, me avise antes de continuar
- Use o DESIGN.md apenas como complemento, nunca como fonte principal

## Arquivos protegidos — NUNCA modifique sem me perguntar antes
- .env
- opencode.json
- AGENTS.md
- DESIGN.md
- PRD.md

## Comportamento geral
- Antes de deletar ou sobrescrever qualquer arquivo existente, me pergunte
- Se não tiver certeza sobre uma regra de negócio, pergunte antes de implementar
- Prefira criar arquivos novos a modificar arquivos grandes de uma vez
```

---

## Passo 7 — Exportar os arquivos do Stitch

No painel do Stitch, com seu design aberto, exporte:

- **DESIGN.md** — design system (cores, tipografia, espaçamentos, componentes)
- **PRD.md** — requisitos e briefing do produto

Salve os dois na raiz do projeto.

---

## Passo 8 — Proteger arquivos sensíveis com .gitignore

```bash
code .gitignore
```

```
# API key e variáveis de ambiente
.env

# Arquivos de design e produto (privados)
DESIGN.md
PRD.md
AGENTS.md
```

Verifique:

```bash
git status
```

Os arquivos listados não devem aparecer.

---

## Passo 9 — Verificar se o MCP está conectado

```bash
export STITCH_API_KEY=suachaverealaaqui
opencode mcp list
```

**Resultado esperado:**

```
┌  MCP Servers
│
●  ✓ stitch connected
│      npx @_davideast/stitch-mcp proxy
│
└  1 server(s)
```

Se aparecer `✓ stitch connected` — está tudo pronto.

---

## Erros que aconteceram e como resolver

### `StitchProxy requires an API key (STITCH_API_KEY)`

A API key não está sendo passada. Exporte antes de rodar:

```bash
export STITCH_API_KEY=suachaverealaaqui
```

### `No MCP servers configured` no `opencode mcp list`

O OpenCode não está lendo o `opencode.json`. Verifique:

1. Se o arquivo existe na raiz do projeto: `cat opencode.json`
2. Se você está na pasta certa: `pwd`
3. Se o `command` é um array: `["npx", "@_davideast/stitch-mcp", "proxy"]`

### `✗ stitch failed — Connection closed`

O servidor reconhece a configuração mas não consegue conectar. Causa mais comum: a API key não está disponível como variável de ambiente. Solução:

```bash
export STITCH_API_KEY=suachaverealaaqui
opencode mcp list
```

### O OpenCode ignorou o MCP e usou só o DESIGN.md

O agente escolheu o caminho mais fácil. Duas soluções:

1. Seja explícita no prompt: _"use o stitch MCP para buscar o design"_
2. Garanta que o AGENTS.md tem a instrução de usar o MCP primeiro

### O OpenCode apagou o `.env` sem avisar

Faltava o campo `permission` no `opencode.json`. Adicione:

```json
"permission": {
  "edit": "ask",
  "bash": "ask"
}
```

### `Configuration is invalid... instructions`

O campo `instructions` precisa ser um array de caminhos de arquivo, não texto:

```json
"instructions": ["AGENTS.md", "DESIGN.md", "PRD.md"]
```

---

## Estrutura final do projeto

```
seu-projeto/
├── opencode.json       ← configuração do OpenCode + MCP
├── .env                ← API key (não vai pro Git)
├── .gitignore          ← protege arquivos sensíveis
├── AGENTS.md           ← regras de comportamento do agente
├── DESIGN.md           ← design system exportado do Stitch
├── PRD.md              ← briefing e requisitos exportados do Stitch
└── src/                ← seu código
```

---

## Referências

- [Google Stitch](https://stitch.withgoogle.com/)
- [Stitch MCP — Repositório oficial](https://github.com/davideast/stitch-mcp)
- [Stitch MCP Setup — Documentação](https://stitch.withgoogle.com/docs/mcp/setup)
- [Google Cloud CLI — Instalação](https://docs.cloud.google.com/sdk/docs/install-sdk?hl=pt-br)
- [OpenCode — MCP Servers](https://opencode.ai/docs/mcp-servers/)
- [OpenCode — Configuração](https://opencode.ai/docs/config/)
- [OpenCode — Rules / AGENTS.md](https://opencode.ai/docs/rules/)
- [Workflow de referência — Stitch + Claude Code](https://pasqualepillitteri.it/en/news/647/google-stitch-mcp-export-claude-code-design-to-code)