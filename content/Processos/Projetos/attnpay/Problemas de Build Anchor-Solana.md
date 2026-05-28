Relatório: Problemas de Build Anchor/Solana
1. Problema Inicial
O usuário reportou conflitos ao rodar anchor build no projeto AttentionPay (Solana).
2. Diagnóstico Inicial
Item	Encontrado
Anchor CLI	0.30.1 (specificado: 0.31.1)
Solana CLI	2.1.0
Rust global	1.95.0 (2026)
Cargo.lock	formato v4 incompatível
3. Tentativas Realizadas
Tentativa 1: Atualizar Anchor CLI
- Instalou Anchor 0.31.1 via avm
- Resultado: Não resolveu
Tentativa 2: Criar config ~/.anchor/
- Criou diretório manualmente
- Ajustou Anchor.toml (adicionou mainnet)
- Resultado: Não resolveu
Tentativa 3: Recriar Cargo.lock
- Removeu Cargo.lock antigo
- Resultado: Novo lock gerado, mas problema persistiu
Tentativa 4: Adicionar package.json
- Criou package.json com dependências Anchor
- Resultado: Apenas preparatório, não resolveu
Tentativa 5: Atualizar Solana CLI (2.1 → 2.2.0)
- Instalou Solana 2.2.0
- Resultado: Não resolveu (mesma toolchain bundlada)
Tentativa 6: Atualizar para stable
- Usou agave-install update
- Resultou em Solana 3.1.15
- Resultado: ** mesmo problema persists **
Tentativa 7: Downgrade Anchor (0.31.1 → 0.30.1)
- Alterou todas as dependências
- Resultado: Não resolveu
Tentativa 8: Forçar nightly
- Setou rustup override nightly
- Resultado: Não resolveu (Anchor internamente usa toolchain Solana)
Tentativa 9: Remover toolchain Solana
- Desinstalou toolchain "solana" do rustup
- Resultado: Não resolveu
4. Descoberta da Causa Raiz
O problema não é a versão do Solana CLI — é a toolchain bundlada.
Componente	Versão Bundlada
Solana 2.1.x	Rust 1.79 (ago/2024)
Solana 2.2.0	Rust 1.79
Solana 3.1.15	Rust 1.79
Todas as versões bundlam Rust 1.79, que é de agosto de 2024. As dependências modernas do crates.io (2025/2026) exigem edition2024, que só existe em Rust 1.85+.
5. Solução Funcional
O cargo build funciona usando a nightly do sistema (Rust 1.97). Isso ocorre porque:
- Não usa cargo-build-sbf (o build tool bundlado)
- Compila para target nativo, não BPF
Output gerado:
libattention_ledger.so (23MB)
libpayout_policy.so   (48MB)  
libreputation_proof.so (23MB)
6. Alternativas Exploradas
Alternativa	Viabilidade
Downgrade crates	❌ Frágil, não sustentável
Clonar/compilar Solana SDK	⚠️ Muito trabalhoso
Pinocchio (zero-dep)	✅ Resolve, mas exige reescrever tudo
cargo build	✅ Funciona, mas output não é BPF
Veredicto Final
Status Atual
- ✅ Desenvolvimento local: FUNCIONAL (cargo build)
- ⚠️ Deploy em rede Solana: BLOQUEADO (toolchain bundlada desatualizada)
Causa
O Solana CLI bundla sua própria toolchain Rust que está 1+ ano atrasada. Este é um problema conhecido do ecossistema (vide issues #3614, #3162 no GitHub).
Soluções Disponíveis
1. Curto prazo: Usar cargo build para desenvolvimento/testing local
2. Médio prazo: Utilizar ambiente de cloud com toolchain já configurada
3. Longo prazo: Esperar atualização da toolchain bundlada (sem previsão)
Recomendação
Para projetos novos em 2026, considerar usar Pinocchio ou SDKs modulares que não dependem da toolchain bundlada do Solana.
Conclusão: O projeto AttentionPay compila localmente com cargo build, mas o deploy em Solana mainnet/devnet requer solução externa até o Solana atualizar suas toolchains bundladas.