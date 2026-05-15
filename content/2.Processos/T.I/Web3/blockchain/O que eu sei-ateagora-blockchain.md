Começando pelo mais básico, sei que blockchain é um tipo de sistema de registro e auditoria de informações (dados) que funciona com base em blocos de registros utilizando relações criptograficas tipo os commits do git que são submetidos a processos de validação por validadores. 

Existem diferentes blockchains que usam diferentes tecnologias, mas todas seguem o mesmo principio de: 
- os registros são públicos e imutáveis
- as blockchain são de rede distribuida
- as blockchain são deterministicas (tipo a matematica clássica é deterministica, a lógica binária é deterministica ou seja o que é aplicado em uma situação também é valido para outra. O dinheiro tipo o dolar não é deterministico pois não é a mesma coisas em todo lugar do mundo;) 
- cada bloco de informação está integralmente relacionado a outro (a parte da imutabilidade)
- Toda e qualquer ação na blockchain custa gas (menos leitura de informações)
- Existem mecanismos de consenso para validar as informações a serem validadas.

### As blockchain que eu conheço são: 

1. Ethereum blockchain 
2. Bitcoin 
3. Klever 

### Ethereum

O ecossistema da ethereum trouxe a tona a existencia da EVM (ethereum virtual machine) que veio para tornar possível rodar programas que executam ações dentro da blockchain. 
E assim começou a saga dos Smart contracts (contratos inteligentes)

### contratos inteligentes
Os contratos inteligentes não pensam sozinhos, mas faz com que combinados sejam automaticamente executados quando as condições são atendidas, ou seja é um contrato programado. Todo contrato inteligente tem algum tipo de custo (real, dindin), eles podem ser aplicados na Mainnet (rede principal que custa tokens verdadeiros, dindin real) ou na Testenet (na rede de teste onde os custos são em faucets, dindin de mentirinhas). Vale lembrar que para fazer o deploy (ou seja, subir pra rede) sempre tera um custo, mas para executar/chamar o contrato inteligente vai ter um custo dependendo do que ele vai fazer, por exemplo a única ação que tem custo 0 é a de ler informações gravadas, o resto sempre vai ter custo como transações ou editar informações.

Os códigos dos contratos inteligentes são públicamente verificaveis (por scanners de cada blockchain, a ethereum por exemplo tem o EtherScan) já que as infos das blockchains precisam ser públicas (ou seja, auditáveis). 

##### Ethereum -> No caso da ethereum a linguagem de programação padrão é o solidity

##### Klever -> No caso da Klever a linguagem padrão é Rust

##### Bitcoin -> No caso do bitcoin eu não faço ideia 


### Ethereum, consenso e taxa de gas
Na Ethereum o mecanismo de consenso atualmente é o de proof of stake (existem varios tipos de mecanismos), onde os validadores são pessoas da comunidade que investiram em tokens e submetem uma quantidade considerável deles A um travamento onde eles são retidos dentro da blockchain (stake), mostrando seu comprometimento com a rede. Assim surge um validador de blocos da blockchain (optimism rollups). As coins (tokens) travadas (em stake) são usadas no processo de validação dos blocos; se os validadores proporem blocos corretamente eles ganham recompensa (parte do gas, a "gorjeta"). Esse mecanismo de proof of stake consome menos eletricidade e é mais rápido do que o proof of work usado no bitcoin por exemplo.

#### Taxa de Gas
A taxa de gas é tipo a energia que gastamos para o corpo funcionar só que aplicando no contexto das blockchains, por isso toda ação tem um custo de gas (gas fee). O gas é calculado de acordo com a quantidade de custo operacional que aquela ação demanda. O Gas é o nome da UNIDADE, ou seja imagine que um contrato inteligente "quer" adicionar uma informação, isso vai custar 3 gas. Já a taxa de gas vai ser o valor do "pacote todo". O custo de gas é previsível, ou seja ele não ocila que nem o valor de moedas economicas como tokens, real ou dólar.  

A taxa (Tx Fee) é calculada assim: 

Tx feee = gas cost * gas price 
(taxa) = (custo de gas) * (preço do gas)

O preço do gas é medido em Gwei. 
1 Gwei = 0.000000001 ether (token da ethereum)
1 Wei = 0.0000000000000000001 ether (são 18 zeros)


Dependendo da velocidade que se quer fazer uma ação ou swap, mais custoso será ($ R$).  


## Mecanismos de consenso
Vale lembrar que existem (nos limites do meu conhecimento) uns dez tipos de mecanismos de consenso: 
1. Proof of work (PoW) 
2. Proof of stake (PoS)
3. Delegated Proof of Stake (DpoS)
4. Proof of Activety (PoA)
5. Proof of Autority (PoA)
6. Proof os Burn (PoB)
7. Proof of Capacity/ Proof of Space
8. Proof of elapsed time 
9. Proof of history 
10. Proof of importance

Ainda não sei como cada um funciona. 

## Layers
Antes de falar de rollups preciso esclarecer o que são layers e quais são eles. Os layer são camadas que fazem parte do funcionamento das blockchains e web3 em geral. As layers são: 
- Layer 3 -> Interfaces, camada de aplicação
- Layer 2 -> Payments/network state channel (escalabilidade)
- Layer 1 -> processo do consenso
- Layer 0 -> P2P network 
- Hardware layer -> a parte fisica da parada

[[blockchains-explicacao1.excalidraw]]

## Rollups 
pelo o que eu entendi são estratégias que definem como/por quem vão ser feitas as validações (consenso). Tipos: 
- Optimism Rollup -> onde há a confiança no agente, como no proof os stake, onde a validação é confiada a um agente que vai ver se a informação é correta ou não (Layer 2). Depois disso vai para a etapa onde outros agentes da comunidade que não necessariamente são validadores podem fazer uma segunda verificação e ver se esta correto; em um período de 7 dias pode haver uma contestação depois disso que não houver nenhuma as informações são registradas permanentemente (L1).  
	 {Nas minhas anotações está especificando que as L2 são mais baratas pq elas encurtam ou resumem o que dever ser armazenado. mas honestamente não internalizei isso de forma que eu possa explicar melhor.}
- Zero Knowledge Rollups -> aqui o agente prova matematicamente que é confiável. Essa solução é mais confiável pois evita erros humanos, porém a maioria usa o optmism porque o ZK é difícil de compreender como funciona por causa da tal "prova matemática", pouca gente entende como funciona a ponto de replicar e aplicar. Mas sem dúvidas é a melhor solução, a solução futura. 

## sla

##### MEMPOOL, “quem paga mais”
Quando você envia uma transação, ela não vai direto para o bloco. Ela entra na mempool que é basicamente uma fila pública de transações pendentes.

O validador (no Proof of Stake) vai escolher quais transações incluir no próximo bloco. E como ele escolhe?

Ele prioriza quem paga mais taxa.  

[[Drawing 2026-02-26 16.18.42.excalidraw]]

A frase “quem paga mais” pertence à camada econômica, mas atua diretamente sobre o consenso. Porque o validador é economicamente incentivado a priorizar maior recompensa.


##### Gas, custo computacional infinito e Turing complete

A EVM (Ethereum Virtual Machine) é Turing complete. Isso significa que teoricamente um contrato pode rodar para sempre. Mas como impedir loops infinitos?

Resposta: gas.

Se o Gas é a unidade que mede o custo computacional e cada operação tem um custo em gas, se ele acabar, a execução para. Portanto, o gas pertence à camada de execução.

Ele é o mecanismo que impede que o fato de ser Turing complete cause colapso.

[[Drawing 2026-02-26 16.18.42.excalidraw]]

Revert

Se uma execução falha, o estado volta ao ponto anterior. Mas o gas gasto não volta, porque o validador já gastou poder computacional processando aquela transação.Isso conecta execução com economia novamente. Se paga pelo uso da máquina, não pelo sucesso do resultado.

 
##### Max gas limit e escassez programada

Cada bloco tem um limite máximo de gas. Isso significa que existe um limite de computação por bloco.

Consequência:  
Bloco tem espaço limitado → Transações competem → Gas sobe.

Isso cria escassez artificial de capacidade computacional. Aqui estamos na interseção entre: Execução (limite técnico) e Economia (oferta e demanda por espaço no bloco)
 
 
##### EIP-1559, inflação e deflação

Aqui é a política monetária do Ethereum. EIP significa Ethereum Improvement Proposal.

Antes do EIP-1559 toda taxa ia para o minerador/validador. Depois do EIP-1559 a taxa base é queimada (burned). Só a gorjeta vai para o validador.

Se muito ETH é queimado → oferta diminui → deflação.

Se pouco é queimado → pode haver inflação dependendo da emissão.

Ou seja, Gas impacta diretamente a oferta monetária do ETH.


Usuários pagam gas →Validador escolhe transações →  
Bloco é produzido →Parte da taxa é queimada →  
Oferta de ETH muda →Economia da rede se ajusta.

Rollups e Layers

Aqui entra escalabilidade.
 
Blocos têm limite de gas.Rede principal é limitada. Rollups executam transações fora da mainnet e publicam apenas provas na camada 1. Isso reduz competição por gas na L1. Portanto: Rollups são uma camada acima da execução principal. Eles aliviam a pressão econômica causada pelo limite de gas.


## Perguntas: 
as maquinas vrtuais são ecossistemas? tipo a kvm e evm são ecossistemas pq são maquinas virtuais ou são ecossistemas e por isso precisam de uma maquina virtual?

O que define um ecossistema?
