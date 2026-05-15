# Quickstart: Google Stitch MCP + OpenCode no Linux

**Objetivo:** Conectar um design do Google Stitch ao OpenCode CLI via MCP, permitindo que o agente de IA leia e use seu design system durante o desenvolvimento.

**Sistema:** Linux x86_64  
**Pré-requisitos:** Node.js/npm, OpenCode CLI e VS Code já instalados.

---

## O que é cada peça

|Ferramenta|Para que serve|
|---|---|
|**Google Stitch**|Gera interfaces e design systems a partir de prompts|
|**Stitch MCP Server**|Ponte entre o Stitch e o agente de código — passa o design automaticamente|
|**Google Cloud CLI (gcloud)**|Necessário apenas para autenticação com a conta Google — não gera custos|
|**OpenCode CLI**|Agente de IA que lê o design e gera/edita o código do projeto|

---

## Passo 1 — Instalar o Google Cloud CLI

> Referência: [Documentação oficial Google Cloud CLI](https://docs.cloud.google.com/sdk/docs/install-sdk?hl=pt-br)

```bash
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

Durante a instalação, responda `Y` para as duas perguntas (adiciona o gcloud ao terminal automaticamente).

**Feche e reabra o terminal**, depois verifique:

```bash
gcloud --version
```

> **Como saber sua arquitetura Linux:** rode `uname -m`. Se retornar `x86_64`, use o arquivo acima. Se retornar `aarch64`, baixe a versão Arm.

---

## Passo 2 — Autenticar no Google

Esses dois comandos valem para todo o computador — podem ser rodados de qualquer pasta:

```bash
gcloud auth login
```

Abre o navegador para login com sua conta Google. Autorize.

```bash
gcloud auth application-default login
```

Abre o navegador novamente. Autorize. Isso permite que o MCP use suas credenciais automaticamente.

> O aviso `WARNING: Cannot find a quota project` que pode aparecer pode ser ignorado — não afeta o uso gratuito.

---

## Passo 3 — Testar o servidor MCP do Stitch

> Referência: [Stitch MCP Setup](https://stitch.withgoogle.com/docs/mcp/setup) | [Workflow de referência](https://pasqualepillitteri.it/en/news/647/google-stitch-mcp-export-claude-code-design-to-code)

Substitua `SUA_API_KEY` pela chave gerada no painel do Stitch:

```bash
STITCH_API_KEY=SUA_API_KEY npx @_davideast/stitch-mcp proxy
```

Se aparecer:

```
[stitch-proxy] Connected to Stitch, discovered 14 tools
[stitch-proxy] Proxy server running
```

Funcionou. Aperte `Ctrl+C` para parar — era só um teste.

---

## Passo 4 — Configurar o OpenCode no projeto

Navegue até a pasta do projeto:

```bash
cd caminho/do/seu/projeto
```

Crie o arquivo `.env` para guardar a API key com segurança:

```bash
code .env
```

Adicione dentro:

```
STITCH_API_KEY=SUA_API_KEY
```

Crie o arquivo de configuração do OpenCode:

```bash
code opencode.json
```

Cole o seguinte conteúdo:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "Sempre consulte o DESIGN.md para decisões visuais e de componentes.",
    "Consulte o PRD.md para entender os requisitos e funcionalidades do projeto."
  ],
  "mcp": {
    "stitch": {
      "type": "local",
      "command": "npx",
      "args": ["@_davideast/stitch-mcp", "proxy"],
      "enabled": true,
      "environment": {
        "STITCH_API_KEY": "{env:STITCH_API_KEY}"
      }
    }
  }
}
```

> O `{env:STITCH_API_KEY}` lê a chave do `.env` automaticamente — sem expor no `opencode.json`.

---

## Passo 5 — Exportar os arquivos do Stitch

No painel do Stitch, com seu design aberto, exporte:

- **DESIGN.md** — design system (cores, tipografia, espaçamentos, componentes)
- **PRD.md** — requisitos e briefing do produto

Salve os dois na raiz do projeto (mesma pasta do `opencode.json`).

O OpenCode vai ler esses arquivos automaticamente durante o desenvolvimento, seguindo as instruções que configuramos no passo anterior.

---

## Passo 6 — Proteger arquivos sensíveis com .gitignore

Para que a API key e os arquivos de design não vão para o GitHub:

```bash
code .gitignore
```

Adicione:

```
# API key e variáveis de ambiente
.env

# Arquivos de design e produto (privados)
DESIGN.md
PRD.md
```

Verifique se funcionou:

```bash
git status
```

Os arquivos listados não devem aparecer.

> Se os arquivos já foram commitados antes, rode `git rm --cached DESIGN.md PRD.md .env` para removê-los do rastreamento sem deletar do computador.

---

## Passo 7 — Usar o OpenCode

Dentro da pasta do projeto:

```bash
opencode
```

O OpenCode vai iniciar, carregar o MCP do Stitch automaticamente e ter acesso ao seu design system. Exemplos do que pedir:

> _"Use o design do Stitch para gerar o componente de login em React"_  
> _"Crie a tela de dashboard seguindo o DESIGN.md"_

---

## Solução de problemas comuns

|Erro|Solução|
|---|---|
|`command not found: gcloud`|Feche e reabra o terminal após a instalação|
|`StitchProxy requires an API key`|Verifique se o `.env` tem a `STITCH_API_KEY` e se o `opencode.json` usa `{env:STITCH_API_KEY}`|
|`Configuration is invalid... instructions`|O campo `instructions` deve ser um array `[ ]`, não texto direto|
|MCP não carrega no OpenCode|Confirme que o `opencode.json` está na raiz do projeto|

---

## Estrutura final do projeto

```
seu-projeto/
├── opencode.json       ← configuração do OpenCode + MCP
├── .env                ← API key (não vai pro Git)
├── .gitignore          ← protege .env, DESIGN.md, PRD.md
├── DESIGN.md           ← design system exportado do Stitch
├── PRD.md              ← briefing e requisitos exportados do Stitch
└── src/                ← seu código
```

---

## Referências

- [Google Stitch](https://stitch.withgoogle.com/)
- [Stitch MCP Setup — Documentação oficial](https://stitch.withgoogle.com/docs/mcp/setup)
- [Google Stitch MCP + Claude Code — Workflow de referência](https://pasqualepillitteri.it/en/news/647/google-stitch-mcp-export-claude-code-design-to-code)
- [Google Cloud CLI — Instalação](https://docs.cloud.google.com/sdk/docs/install-sdk?hl=pt-br)
- [OpenCode — MCP Servers](https://opencode.ai/docs/mcp-servers/)
- [OpenCode — Configuração](https://opencode.ai/docs/config/)
- [stitch-mcp — Repositório GitHub](https://github.com/davideast/stitch-mcp)