Perfeito. Então o , do mais visual para o mais lógico. Deixa eu explicar a lógica antes dos prompts:

[[]]

---

## Por que essa ordem?

O OpenCode precisa primeiro **entender o que existe** antes de **construir**. Se você pedir para gerar tudo de uma vez, ele vai tomar decisões arbitrárias que você vai ter que corrigir depois. A ordem abaixo garante que cada etapa valida a anterior.

---

## As 4 fases e os prompts

---

### Fase 1 — Reconhecimento (primeiro prompt)

O agente lê tudo que você preparou e te devolve um plano. Você valida antes de ele escrever uma linha de código.

```
Leia o DESIGN.md e o PRD.md deste projeto e use as ferramentas do 
Stitch MCP para inspecionar o design atual. 

Com base nisso, me devolva:
1. Um resumo do que você entendeu sobre o produto
2. A lista de telas/páginas que precisam ser criadas
3. A lista de componentes reutilizáveis que identificou
4. As entidades de dados que vão precisar de backend (ex: usuário, pedido, etc)

Não escreva código ainda. Quero revisar seu entendimento primeiro.
```

> Você lê o que ele devolveu, corrige o que estiver errado, e só então avança.

---

### Fase 2 — Estrutura do projeto

Depois que o plano está alinhado, ele monta o esqueleto.

```
O plano está correto. Agora crie a estrutura de pastas e arquivos 
do projeto em React. Inclua:
- Organização de componentes, páginas e rotas
- Arquivo de configuração de rotas
- Estrutura de pastas para o backend (API)
- Arquivo de variáveis de ambiente (.env.example)

Não implemente a lógica ainda, só crie a estrutura.
```

---

### Fase 3 — Frontend por tela

Uma tela por vez, começando pela mais importante do seu produto.

```
Implemente a tela [nome da tela] seguindo exatamente o DESIGN.md. 
Use o Stitch MCP para buscar os detalhes visuais dessa tela.
Crie os componentes necessários e conecte na rota correta.
Dados podem ser mockados por enquanto. se já existir componentes que estejam nessa pagina não crie outro novamente, apenas reutilize. 

---

implemente a tela de Boas-vindas SEGUINDO O Stitche MCP e Design.md pra buscar os detalhes visuas da tela. crie os componentes necessários e conecte na rota correta. se já existir componentes que estejam nessa pagina não crie outro novamente, apenas reutilize. 

---

Implemente a tela [nome da tela] seguindo exatamente o DESIGN.md. 
Use o Stitch MCP para buscar os detalhes visuais dessa tela.
Crie os componentes necessários e conecte na rota correta.
Dados podem ser mockados por enquanto. se já existir componentes que estejam nessa pagina não crie outro novamente, apenas reutilize. 

---

Implemente a tela Configurações seguindo fielmente o Stitch MCP .
Crie os componentes necessários e conecte na rota correta.
Dados podem ser mockados por enquanto. se já existir componentes que estejam nessa pagina não crie outro novamente, apenas reutilize. 
```
 
Implemente a seção de "Editar avatar" da tela "perfil simplificado" identico ao MCP do stitch, implemente ele logo a cima da seção "Conta" da página configurações que está dentro de profile. seguindo as boas práticas da componentização. faça com que a imagem de perfil ou avatar seja clicavel. 

Implemente a tela "Seleção de Avatar" seguindo fielmente o Stitch MCP, e as boas práticas e lógica da componentização. Crie os componentes para que o crescimento ou modificação futura sejam rapidamente implementados, reutilize os componentes que já existam e que façam sentido, e conecte na rota correta. essa tela deve ser ativada quando o usuario clicar no avatar ou imagem de perfil na seção "Editar avatar" em configurações. 


```
O frontend da tela [nome] está aprovado. Agora:
1. Crie a API necessária para essa tela (rotas, controllers, validações)
2. Substitua os dados mockados pela chamada real à API
3. Adicione tratamento de erro e estados de loading
```

---

## O que fazer agora

Rode o **prompt da Fase 1** no OpenCode e me manda o que ele devolveu. A gente valida juntos o entendimento dele antes de você avançar para a Fase 2 — esse passo evita retrabalho lá na frente.

---
Análise

1. Camada de Componentes UI (Atoms/Molecules)
Componente	Descrição	Como Fazer
Logo	Logo AttnPay reutilizável	Extrair de Header.tsx com props para size/variant
SocialButton	Botões Google/Wallet no Login/Signup	Criar componente com variant (google, phantom, walletconnect)
PasswordStrength	Indicador de força de senha	Extrair de ForgotPassword.tsx
VerificationInput	Input de 6 dígitos	Extrair de Verification.tsx como componente isolated
LanguageToggle	PT/EN toggle	Já existe IdiomButton, mas não está padronizado
FormField	Input com label + erro + ícone	Unificar Input.tsx com patterns repetidos
ActionButton	Botões primários/secondários do auth	Consolidar estilos repetidos
Divider	"OU CONTINUE COM" lines	Criar componente separado

2. Componentes de Layout (Organisms)
Componente	Descrição	Como Fazer
AuthLayout	Layout base para Login/Signup/ForgotPassword	Extrair header + background + footer repetidos
DashboardLayout	Layout com Header + BottomNav + bg gradient	Base para Dashboard, Profile, Support
SettingsSection	Sections de configurações	Criar componente com título + children
TimelineItem	Evento na timeline do Support.tsx	Extrair de Support.tsx

3. Hooks Customizados
Hook	Responsabilidade	Como Fazer
useAuth	Gerenciar estado de autenticação	Criar contexto + hooks (login, logout, isAuthenticated)
useLocalStorage	Persistência de dados	Hook genérico para localStorage
useForm	Gerenciar estados de formulários	Hook reutilizável para inputs
useTimer	Contagem regressiva	Usado em Verification.tsx
useNavigateOnSuccess	Redirecionamento após actions	Reduzir boilerplate
usePasswordStrength	Validar força da senha	Extrair lógica de ForgotPassword.tsx

4. Context Providers
Provider	Descrição	Estado
AuthProvider	Estado global de autenticação	user, isAuthenticated, login(), logout()
ThemeProvider	Gerenciamento de tema (dark mode)	theme, toggleTheme()
I18nProvider	Internacionalização	language, setLanguage(), translations

5. Rotas Protegidas
Criar sistema de rotas com auth guards:
// src/routes/ProtectedRoute.tsx
const ProtectedRoute = ({ children }) => {
  const { isAuthenticated } = useAuth();
  return isAuthenticated ? children : <Navigate to="/login" />;
};

6. Pages a componentizar
Page	Componentes a Extrair
Login.tsx	AuthLayout, SocialButton, PasswordInput
Signup.tsx	AuthLayout, FormFields
Verification.tsx	VerificationInput, Timer, ResendButton
ForgotPassword.tsx	AuthLayout, PasswordStrength, PasswordInput
Dashboard.tsx	WelcomeCard, AttentionScoreCard, WeeklyChart
Profile.tsx	ProfileTabs, PersonalDataCard, WalletCard
Support.tsx	TimelineItem, FloatingActionButton
SettingsTab.tsx	SettingsSection, SettingsItem

src/
├── components/
│   ├── ui/                    # Atoms
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   ├── Logo.tsx
│   │   ├── Avatar.tsx
│   │   ├── Divider.tsx
│   │   ├── LanguageToggle.tsx
│   │   ├── PasswordStrength.tsx
│   │   ├── VerificationInput.tsx
│   │   └── SocialButton.tsx
│   ├── layout/                # Organisms
│   │   ├── AuthLayout.tsx
│   │   ├── DashboardLayout.tsx
│   │   ├── Header.tsx
│   │   ├── BottomNav.tsx
│   │   └── SettingsSection.tsx
│   ├── dashboard/             # Dashboard specific
│   │   ├── WelcomeCard.tsx
│   │   ├── AttentionScore.tsx
│   │   ├── WeeklyChart.tsx
│   │   └── MetricsRow.tsx
│   ├── profile/
│   │   ├── ProfileHeader.tsx
│   │   ├── ProfileTabs.tsx
│   │   ├── PersonalDataCard.tsx
│   │   └── WalletCard.tsx
│   ├── auth/
│   │   ├── LoginForm.tsx
│   │   ├── SignupForm.tsx
│   │   └── PasswordRecoveryForm.tsx
│   └── support/
│       ├── Timeline.tsx
│       └── TimelineItem.tsx
├── hooks/
│   ├── useAuth.ts
│   ├── useForm.ts
│   ├── useTimer.ts
│   ├── useLocalStorage.ts
│   └── usePasswordStrength.ts
├── context/
│   ├── AuthContext.tsx
│   ├── ThemeContext.tsx
│   └── I18nContext.tsx
├── pages/
│   └── (cada page mais simples, delegando para componentes)
├── routes/
│   ├── index.tsx
│   └── ProtectedRoute.tsx
├── utils/
│   ├── formatters.ts          # formatCurrency, formatDate
│   ├── validators.ts          # password validation
│   └── constants.ts
└── styles/
    └── theme.ts
---
Deseja que eu implemente alguma parte específica deste plano? Posso começar por:
1. Hooks e Context (base para tudo)
2. Componentes de UI (mais rápido de fazer)
3. Layouts componentizados
4. Proteção de rotas
Qual direção prefere?

---
❌ O QUE AINDA PRECISA SER IMPLEMENTADO
1. Páginas de Autenticação (Login, Signup, Verification, ForgotPassword, Welcome)
Problemas:
- Header repetido em cada página (verbatim copy-paste de ~15 linhas)
- Background effects duplicados (divs com blur, gradientes)
- Footer duplicado
- Form fields inline (não usam os novos componentes UI)
- Social buttons inline (não usam SocialButton)
- Divider inline (não usa Divider)
- Botões de submit inline (não usam ActionButton)
- Lógica de timer inline em Verification (não usa useTimer hook)
- Lógica de password strength inline em ForgotPassword (não usa usePasswordStrength)
- Language toggle manual (não usa LanguageToggle)
O que criar/refatorar:
- Criar LoginForm.tsx como componente isolado dentro de src/components/auth/
- Criar SignupForm.tsx similar
- Criar AuthPageWrapper.tsx para envolver páginas de auth com header/footer padrões
- Aplicar AuthLayout que já foi criado
---
2. Dashboard
Problemas:
- WelcomeCard inteiro inline (ola, uptime, consumo) - 50 linhas num bloco só
- AttentionScore (SVG progress ring + métricas) - 40 linhas inline
- WeeklyChart (barras da semana) - inline
- Background gradient hardcoded no style prop
- Não usa os hooks disponíveis para dados
O que criar:
- src/components/dashboard/WelcomeCard.tsx - Extrair para componente
- src/components/dashboard/AttentionScore.tsx - O círculo SVG + métricas
- src/components/dashboard/WeeklyChart.tsx - As barras de dias
- src/components/dashboard/MetricsRow.tsx - Uptime/Consumo row
---
3. Profile
Problemas:
- Tabs navigation inline (~15 linhas)
- Personal data card inline
- Wallet card inline (o botão de conectar wallet)
- Não integra com AuthContext para dados do usuário
O que criar:
- src/components/profile/ProfileTabs.tsx - Os botões de tab
- src/components/profile/PersonalDataCard.tsx - Dados pessoais
- src/components/profile/WalletCard.tsx - Cartão de wallet conectada
- Integrar useAuth() para exibir dados reais do usuário
---
4. Support
Problemas:
- Timeline events array inline na página
- TimelineItem componente não existe (lógica toda inline)
- Feature cards (criptografia, pagamentos, identidade) inline
- FAB (floating action button) inline
O que criar:
- src/components/support/Timeline.tsx - Container da timeline
- src/components/support/TimelineItem.tsx - Item individual
- src/components/support/FeatureCard.tsx - Cards de benefícios
- src/components/common/FloatingActionButton.tsx - O botão flutuante
---
5. App.tsx
Problema crítico:
- Não envolve a aplicação com os Providers criados
- Os hooks/contexts (AuthProvider, ThemeProvider, I18nProvider) não estão sendo utilizados
O que fazer:
- Importar e envolver AppRoutes com os três providers
- Isso habilitará autenticação real, tema e i18n em todas as páginas
---
📋 Resumo do Que Falta Para Escala Fluida
1. App.tsx - Integrar Providers (Auth, Theme, I18n)
2. Páginas Auth - Aplicar AuthLayout + componentes UI + hooks
3. Dashboard - Extrair WelcomeCard, AttentionScore, WeeklyChart
4. Profile - Extrair tabs e cards, integrar useAuth
5. Support - Criar Timeline components
6. Routes - Criar ProtectedRoute para páginas autenticadas
Cada um dessesitems tem aproximadamente 15-40 linhas de código repetido por página que podem ser extraídos para componentes reutilizáveis. Com essas refatorações, cada página ficaria com ~30-50 linhas de código puro de lógica específica da página, sem marcação UI repetida.