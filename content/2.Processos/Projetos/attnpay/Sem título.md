Entendi. O projeto ainda está na fase de especificação - não há código/ferramentas do Stitche MCP configuradas ainda.
---
1. Resumo do Produto
AttnPay é uma plataforma financeira e de segurança focada em proteger a atenção e ativos do usuário. Combina:
- Gestão financeira tradicional (PIX)
- Capacidades Web3 (Crypto/Wallets)
- Autenticação segura com biometria e 2FA
Design: Lumina Glass - glassmorphism dark-mode com acentos neon (violeta/cyan) e tipografia Inter.
---
2. Telas/Páginas a Criar
#	Tela	Descrição
1	Loading/Splash	Ponto de entrada imersivo com branding
2	Welcome	Tela intermediária (Login / Cadastro)
3	Login	Autenticação com email/senha + 2FA
4	Sign-up (Cadastro)	Registro com validação de campos
5	Esqueci a Senha	Recuperação de senha
6	Dashboard	Visão geral, saldo, atividades recentes
7	Financeiro Detalhado	Transações PIX, gráfico evolução patrimonial
8	Perfil	Dados pessoais, redes sociais vinculadas
9	Configurações	Segurança, aparência, idioma, conexões
10	Suporte	Help center e chat com IA
11	Conectar Wallet	Integração Phantom/WalletConnect
---
3. Componentes Reutilizáveis
Componente	Descrição
GlassCard	Container com backdrop-blur, borda translúcida
Button	Primary, Secondary, Ghost (estilos variados)
Input	Campos com estados focus/error/success
BottomNav	Navegação mobile fixa com efeito glass
Header	Cabeçalho com logo e ações
TransactionItem	Linha de transação (ícone + valor + data)
BalanceCard	Card de saldo com grafismo
Chart	Gráfico de evolução patrimonial
Modal	Overlays para ações
Avatar	Foto de perfil
Toggle	Switch para opções
Loader	Loading states
---
4. Entidades de Dados (Backend)
Entidade	Campos Principais
User	id, email, name, phone, avatar, language, createdAt
Auth	userId, passwordHash, 2FAEnabled, biometricData
Wallet	id, userId, type (PIX/Crypto), balance, address
Transaction	id, walletId, type, amount, status, timestamp, metadata
WalletConnection	id, userId, provider, address, connectedAt
SupportTicket	id, userId, subject, messages, status, createdAt
Settings	userId, theme, language, notifications
---
Revisão necessária? Algo que eu tenha entendido incorretamente ou que precise ser adicionado?