Situação atual: frontend no código porém sem backend básico. backend de contratos pronto e deployado. 

intenção: fazer o backend básico do front. fazer git subtree com o backend dos contratos inteligentes. testar. colocar no github.

# Problemas encontrados na alnálise critica:
1. ```Antes de qualquer `subtree add`, isso precisa ser limpo do histórico do `attentionpay-frontend` — seja com um `git filter-repo` ou commitando um `.gitignore` correto e removendo os arquivos. Se não fizer isso, vai importar lixo junto com o código.``` já coloquei no gitignore, porém como faço para limpar do histórico?

2. ``` Conflito de estrutura: Next.js vs Vite. O `app/` atual no repo principal (backend-blockchain) é Next.js. O frontend do outro repo é Vite + React. São frameworks diferentes com estruturas de build, roteamento e configuração distintas. Quando você "substituir" o `app/`, precisará garantir que os scripts no `package.json` raiz, o `vercel.json`, e qualquer CI/CD apontem para o novo setup. O `vercel.json` na raiz do `catitodev/attentionpay` precisa ser revisado para refletir isso.``` Preciso de um passo a passo de como fazer na hora para não esquecer de fazer nada. 

3.  ``O `attentionpay-frontend` tem uma pasta `backend/`. Essa pasta contém o que exatamente? Backend em Node/Express/Fastify? Se sim, você vai ter que decidir se ela entra dentro do `app/` futuro, ou se vai para uma pasta de nível raiz separada (tipo `api/` ou `server/`). Isso afeta a estratégia do subtree — você não quer trazer o repo inteiro como subtree e aí ter `attentionpay/app/backend/` numa estrutura estranha.`` com base no que que faço essa escolha, o que muda para cada?

4.  ```Se a intenção é que o `attentionpay-frontend` continue sendo desenvolvido em paralelo e depois sincronizado, o subtree faz sentido. Mas se a ideia é só migrar e encerrar o repo antigo, um simples `cp -r` dos arquivos relevantes + commit limpo é mais honesto e menos problemático.``` o que seria cp -r?

5. `` Dois owners, dois repos públicos. `catitodev` é o dono do repo principal. `iaiamaga` é o dono do frontend. Para fazer `subtree add` apontando para um remote externo, qualquer um dos dois pode executar o comando — mas quem vai ser o "repo canônico" daqui para frente? Precisa alinhar isso antes, porque isso afeta quem tem write access, quem faz deploy, e onde fica o source of truth.`` eu pensei em por no calangoflux. 

---

# O que falta para finalizar backend do frontend
[[Analise-backend]]

[[Logica-de-negocio-programacao]]