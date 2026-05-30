---
about: diário de bordo sobre aprendendo rust com rustlings
created: 2026-05-29 15:27:56.5656-03:00
modified: 2026-05-29 17:08:39.3939-03:00
draft: false
tags:
  - "#diariodebordo"
---
## Perguntas norteadoras

1. O que travou?
2. O que tentei fazer?
3. O que ficou mais claro ou mais confuso?

## Respostas
1. travei no começo. depois travei porque eu fiquei batendo a cabeça porque fiquei focando só na idealização da primeira hipótese que criei. 
2. o problema era que eu precisava resolver o compilador, e para isso precisava deixar o assert_eq!(vec0); assert_eq!(vec1); DISPONIVEIS AO mesmo tempo. porém estava dando um erro. pensei de cara em concatenar o vec0 e vec1.... no meio das tentativas os erros foram mudando, em um deles apareceu um aviso sobre .clone() e eu add .clone()... porém ainda estava dando erro. pesquisei nos docs, mas nao encontrava pistas úteis. ai pivotei tudo e refiz. desisti e falei c claude. depois vi que era literalmente só add clone() mesmo. 
3. ficou mais claro a questão de ownership. outro problema depois desse tb foi sobre ownership. é ainda meio estranho pensar nisso, mas deu para entender melhor como pensar e como é preciso pensar nisso toda vez que estiver avaliando um código. 
