A missão de hoje é entender como usar o sdk da klever (e a lógica geral da utilização de sdks) Klever-connect (https://github.com/klever-io/klever-connect) e talvez por ventura sacar das kapps também. 

O objetivo final é montar uma página de contribuições (ou como chamam: donation page mas não curto o termo doação) que possa ser reutilizável dentro de projetos para que as pessoas possam contribuir depositando klv's numa carteira. Tudo isso usando a klever connect e as kapps. 

## Klever connect, o que é? 

JavaScript/TypeScript SDK for Klever Blockchain - A comprehensive Web3 library inspired by ethers.js, CosmJS, and 
@solana/web3.js

### Key Features
- 🔐 Offline Transaction Building - Build and sign transactions without network access using Proto encoding
- 📦 Modular Architecture - Import only what you need, tree-shakeable design
- 🔷 Full TypeScript Support - First-class TypeScript with branded types for safety
- ⚡ Modern Async/Await API - Clean, promise-based interfaces
- 🔌 Multiple Wallet Support- Hardware, browser extension, and mobile wallets
- ⚛️ React Ready - Built-in hooks and components for dApps
- 🛡️ Security First - Audited cryptographic operations
- 📱 Works Everywhere - Node.js, browsers, React Native


# Projeto: Página de contribuições em KLV 

Pensei no frontend ser parecido com o klever scan ou klever explore. Quero usar React Native e lógico typescript (só pq é oq ando mais usando).  

A ideia é que a aplicação reconheça se o navegador do usuário tem a klever wallet quando o usuário clicar no botão "Connect klever wallet", daí vai pedir para conectar com a autenticação e reconhecer a carteira e acessa-la (trabalho do SDK), essa vai ser a carteira do remetente, a carteira de quem recebe vai ser configurada via código. A aplicação deve reconhecer se a carteira é valida ou não, caso não seja retorne erro ao usuário (wallet inválida). 

A contribuição em KLVs pode ser pela mainnet ou testnet.

O usuário deve poder escrever a quantidade de klv que quer enviar, a aplicação deve reconhecer se o input é válido ou não e sinalizar com erro (valor inválido) caso não seja. Ao clicar no botão de enviar a quantidade de klv digitada vai ser enviada para a carteira configurada como recebedor. 


--- ok, eu ja tentei fzr um com ia (queridinho claude), mas não saquei mt coisa não. vou explorar mais agora para ver se entendo oq ta rolando e oq está faltando.

bom, primeira coisa, não está em tsx, é que não estou familiarizada com js rsrsrsrs.... fui de lebre e pulei umas coisas antes de ter base. mas blz 

achei estranho pq não tem nem um import da klever connect até agr.

pelo o que entendi ele não utilizou o klever connect... nem kapps. apenas criou tudo na "mão"????

## No final 
tive que tirar a parte do klever connect pq tentei juntar dois sdks: kelver connect e klever sdk web, mas estavam dando conflito ent tirei o klever connect..............

repo: 

## dicionário - termos/conceitos novos
- **Payload** refers to the essential data or content being transmitted in a communication, whether in computing, networking, or cybersecurity.  It represents the **core information** that the sender intends to deliver, distinct from metadata, headers, or protocol overhead.
- 