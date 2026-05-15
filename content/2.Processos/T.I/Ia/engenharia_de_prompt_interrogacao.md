[[entendendo_notebooklm]]

- 16 prompts que funcionam: https://gestgov.discourse.group/t/guia-de-prompts-para-notebooklm/34215
- Help Center oficial notebooklm: https://support.google.com/notebooklm?sjid=4048399782211247528-SA#topic=16164070 
- https://multex.io/prompts

# MÓDULO 5 — Engenharia de Prompt Aplicada ao NotebookLM (Nível Avançado)

## 1. A anatomia real de um prompt estratégico

Um prompt forte tem cinco camadas estruturais:

1. Delimitação de universo
    
2. Definição de papel
    
3. Objetivo cognitivo
    
4. Critérios de qualidade
    
5. Formato de saída
    

Se você ignora qualquer uma dessas, a resposta perde precisão.

Vamos dissecar.

---

## 2. Delimitação de Universo

No NotebookLM isso é crucial.

Você deve sempre indicar:

– Use apenas as fontes fornecidas  
– Considere apenas o capítulo X  
– Ignore interpretações externas

Exemplo fraco:

> Resuma o texto.

Exemplo forte:

> Usando exclusivamente as fontes fornecidas, analise o capítulo 2 e identifique os argumentos centrais e suas evidências de suporte.

Você está restringindo o espaço inferencial.

Isso reduz ruído e aumenta precisão.

---

## 3. Definição de Papel (Role Framing)

Você pode fazer o modelo assumir papéis analíticos específicos.

Exemplos:

– Atue como revisor acadêmico rigoroso  
– Atue como examinador de concurso  
– Atue como crítico lógico  
– Atue como professor especialista na área

Mas atenção: o papel deve ter função operacional, não decorativa.

Errado:

> Seja um grande pensador.

Correto:

> Atue como avaliador técnico e identifique inconsistências metodológicas no argumento apresentado.

Você está definindo lente analítica.

---

## 4. Objetivo Cognitivo

Esse é o núcleo.

Pergunte-se:  
Você quer que ele:

– Extraia?  
– Compare?  
– Classifique?  
– Detecte falhas?  
– Reconstrua argumento?  
– Gere perguntas?  
– Simule objeções?

Exemplo:

> Compare as definições de energia apresentadas nas seções 1 e 4 e identifique divergências conceituais.

Isso é muito mais sofisticado do que “explique energia”.

---

## 5. Critérios de Qualidade

Essa é a camada ignorada pela maioria.

Você deve dizer como a resposta deve ser avaliada.

Exemplos:

– Seja tecnicamente rigoroso  
– Evite simplificações excessivas  
– Indique ambiguidades  
– Diferencie fato de interpretação  
– Cite trechos das fontes

Isso força profundidade.

---

## 6. Formato de Saída

Forma muda qualidade.

Exemplos de formatação estratégica:

– Estrutura hierárquica  
– Tabela comparativa  
– Lista numerada com subtópicos  
– Mapa conceitual descritivo  
– Argumento reconstruído em silogismos

Se você não define formato, ele escolhe por você — e geralmente escolhe superficial.

---

# Agora vamos aplicar isso na prática

Imagine que você subiu um PDF de Físico-Química.

Prompt superficial:

> Explique molaridade.

Prompt de engenharia real:

> Usando exclusivamente as fontes fornecidas, explique o conceito de molaridade considerando que o leitor já compreende concentração comum e densidade.  
> Diferencie claramente molaridade de concentração comum.  
> Identifique possíveis erros conceituais que estudantes costumam cometer.  
> Estruture a resposta em três partes: definição formal, comparação conceitual e armadilhas frequentes.

Percebe a diferença?

Você está programando a análise.

---

# Técnicas Avançadas Pouco Ensinadas

Agora vamos para o que quase ninguém fala.

## Técnica 1 — Prompt em Camadas

Primeiro você pede estrutura.  
Depois pede aprofundamento.

Exemplo:

1ª interação:

> Identifique os conceitos centrais do capítulo.

2ª interação:

> Agora aprofunde apenas o conceito 3, detalhando pressupostos implícitos.

Isso cria encadeamento cognitivo.

---

## Técnica 2 — Prompt de Contradição

Peça para ele tentar refutar o próprio texto.

> Identifique possíveis fragilidades ou pontos que poderiam ser criticados por um especialista.

Isso eleva sua capacidade argumentativa.

---

## Técnica 3 — Prompt de Reconstrução Lógica

> Reconstrua o argumento principal do capítulo na forma de premissas numeradas e conclusão explícita.

Isso é extremamente poderoso para filosofia, direito, redação, humanas.

---

## Técnica 4 — Prompt de Lacuna Epistêmica

> Quais conceitos este texto pressupõe que o leitor já saiba, mas não explica?

Isso revela o que você precisa estudar fora.

---

# Agora vamos aprofundar ainda mais

A engenharia de prompt tem três níveis de maturidade:

Nível 1 — Pergunta direta  
Nível 2 — Pergunta estruturada  
Nível 3 — Arquitetura de investigação

No nível 3, você pensa em sequência.

Exemplo de arquitetura completa:

Etapa 1 — Estrutura  
Etapa 2 — Conceitos centrais  
Etapa 3 — Dependências  
Etapa 4 — Fragilidades  
Etapa 5 — Aplicações

Você transforma o NotebookLM em sistema de análise progressiva.

---

Agora eu quero te provocar intelectualmente:

Quando você sobe um PDF novo, qual é seu primeiro movimento mental?  
Você pensa “quero entender isso” ou “quero mapear como isso foi construído”?

Essa diferença define se você será usuária ou estrategista.

