
Análise Completa: AttnPay - De Mock Data para Produção
📋 RESUMO EXECUTIVO
O projeto possui backend disconnected - existe estrutura Express mas não está integrado ao frontend. Toda a aplicação usa dados hardcoded/mockados.

🔴 PRIORIDADE CRÍTICA (Blocos que impedem funcionamento)
1. AUTENTICAÇÃO (Auth Context)
Item	Status	Ação Necessária
Context de Auth	❌ Não existe	Criar AuthContext com provider
JWT Storage	❌ Não existe	Implementar localStorage/sessionStorage
Protected Routes	❌ Não existe	Criar HOC ou componente ProtectedRoute
Logout	❌ Não existe	Implementar clearToken + redirect
Arquivos a criar/modificar:
- src/contexts/AuthContext.tsx (NOVO)
- src/components/auth/ProtectedRoute.tsx (NOVO)
- src/App.tsx - adicionar AuthProvider
- src/pages/Login.tsx - integrar API real
- src/pages/Signup.tsx - integrar API real
2. SERVIÇO DE API (HTTP Client)
Item	Status	Ação Necessária
API Service	❌ Não existe	Criar axios instance com interceptors
Auth Interceptor	❌ Não existe	Adicionar Bearer token automático
Error Handling	❌ Não existe	Padronizar tratamento de erros
Arquivo a criar:
- src/services/api.ts (NOVO) - axios instance configurada
3. STATE MANAGEMENT
Item	Status	Ação Necessária
Global State	❌ Não existe	Implementar Context API ou Zustand
Data Fetching	❌ Não existe	Criar custom hooks ou usar React Query
Decisão necessária:
- Opção A: Context API (built-in, simples)
- Opção B: Zustand (mais leve, moderno)
- Opção C: React Query + Context (melhor para API)
---
🟡 PRIORIDADE ALTA (Funcionalidades ausentes)
4. BACKEND - Banco de Dados Real
Item	Status	Ação Necessária
In-memory store	⚠️ Temporário	Trocar por PostgreSQL/MySQL/MongoDB
Conexão BD	❌ Não existe	Configurar ORM (Prisma/Drizzle/TypeORM)
Migrations	❌ Não existe	Criar schema de migrations
5. BACKEND - Endpoints Incompletos
Endpoint	Status	Ação
GET /api/attention/metrics	❌ Não existe	Criar para Dashboard
GET /api/attention/weekly	❌ Não existe	Criar para gráficos
PATCH /api/users/settings	❌ Não existe	Criar para Preferências
GET /api/support/tickets	❌ Não existe	Criar para Support
POST /api/support/messages	❌ Não existe	Criar para Chat
6. FRONTEND - Pages com Mock Data
Página	Dados Mockados	Ação
Dashboard	weeklyData, user metrics, attention score, points	Criar hook useAttentionMetrics
Profile	personalData, connectedWallet, username	Criar hook useUserProfile
ProfileHeader	"Alex Nova" hardcoded	Buscar do AuthContext
Support/SupportTab	timelineEvents, documents	Criar hook useSupportTickets
ConnectWallet	mock address	Integrar com Phantom/WalletConnect SDK
7. CONECTAR FRONTEND ↔ BACKEND
Item	Status	Ação
CORS Config	⚠️ Básico	Configurar origins permitidas
Vite Proxy	❌ Não configurado	Adicionar em vite.config.ts
API URL	❌ Não configurado	Definir .env com VITE_API_URL
---
🟢 PRIORIDADE MÉDIA (Melhorias)
8. VALIDAÇÃO DE FORMULÁRIOS
Página	Status	Ação
Login	⚠️ Básico	Adicionar Zod + react-hook-form
Signup	⚠️ Básico	Validar email, senha forte, confirma
Forgot Password	⚠️ Básico	Validar formato email
9. FEEDBACK UI (Loading/Error States)
Item	Status	Ação
Loading spinners	❌ Não existe	Criar LoadingSpinner component
Error states	❌ Não existe	Criar ErrorBoundary + ErrorMessage
Toast notifications	❌ Não existe	Integrar react-toastify ou similar
10. RESPONSIVIDADE & ACCESSIBILITY
Item	Status	Ação
Breakpoints	⚠️ Parcial	Revisar mobile-first
ARIA labels	❌ Ausente	Adicionar para leitores de tela
Focus states	❌ Ausente	Implementar navegação por teclado

---
🔵 INFRAESTRUTURA (Deploy)
11. CONFIGURAÇÕES DE AMBIENTE
Arquivo	Status	Ação
.env (Frontend)	⚠️ Parcial	Definir VITE_API_URL, VITE_WS_URL
.env (Backend)	⚠️ Parcial	JWT_SECRET, DB_URL, PORT
.env.example	❌ Não existe	Documentar variáveis necessárias
12. PIPELINE CI/CD
Item	Status	Ação
Build script	✅ Existe	npm run build
Type check	✅ Existe	Verificar npm run typecheck
Lint	❌ Não configurado	Adicionar ESLint + Prettier
Testes	❌ Não existe	Configurar Vitest/Jest
13. WEBSOCKET (Tempo Real)
Item	Status	Ação
Socket.io	❌ Não configurado	Para notifications em tempo real
Real-time updates	❌ Não existe	Para Attention Score atualizar

---
🔴 PRIORIDADE CRÍTICA (Blocos que impedem funcionamento)
1. AUTENTICAÇÃO (Auth Context)
Item	Status
Context de Auth	❌ Não existe
JWT Storage	❌ Não existe
Protected Routes	❌ Não existe
Logout	❌ Não existe
Arquivos a criar/modificar:
- src/contexts/AuthContext.tsx (NOVO)
- src/components/auth/ProtectedRoute.tsx (NOVO)
- src/App.tsx - adicionar AuthProvider
- src/pages/Login.tsx - integrar API real
- src/pages/Signup.tsx - integrar API real
2. SERVIÇO DE API (HTTP Client)
Item	Status
API Service	❌ Não existe
Auth Interceptor	❌ Não existe
Error Handling	❌ Não existe
Arquivo a criar:
- src/services/api.ts (NOVO) - axios instance configurada
3. STATE MANAGEMENT
Item	Status
Global State	❌ Não existe
Data Fetching	❌ Não existe
Decisão necessária:
- Opção A: Context API (built-in, simples)
- Opção B: Zustand (mais leve, moderno)
- Opção C: React Query + Context (melhor para API)
---
🟡 PRIORIDADE ALTA (Funcionalidades ausentes)
4. BACKEND - Banco de Dados Real
Item	Status
In-memory store	⚠️ Temporário
Conexão BD	❌ Não existe
Migrations	❌ Não existe
5. BACKEND - Endpoints Incompletos
Endpoint	Status
GET /api/attention/metrics	❌ Não existe
GET /api/attention/weekly	❌ Não existe
PATCH /api/users/settings	❌ Não existe
GET /api/support/tickets	❌ Não existe
POST /api/support/messages	❌ Não existe
6. FRONTEND - Pages com Mock Data
Página	Dados Mockados
Dashboard	weeklyData, user metrics, attention score, points
Profile	personalData, connectedWallet, username
ProfileHeader	"Alex Nova" hardcoded
Support/SupportTab	timelineEvents, documents
ConnectWallet	mock address
7. CONECTAR FRONTEND ↔ BACKEND
Item	Status
CORS Config	⚠️ Básico
Vite Proxy	❌ Não configurado
API URL	❌ Não configurado
---
🟢 PRIORIDADE MÉDIA (Melhorias)
8. VALIDAÇÃO DE FORMULÁRIOS
Página	Status
Login	⚠️ Básico
Signup	⚠️ Básico
Forgot Password	⚠️ Básico
9. FEEDBACK UI (Loading/Error States)
Item	Status
Loading spinners	❌ Não existe
Error states	❌ Não existe
Toast notifications	❌ Não existe
10. RESPONSIVIDADE & ACCESSIBILITY
Item	Status
Breakpoints	⚠️ Parcial
ARIA labels	❌ Ausente
Focus states	❌ Ausente
---
🔵 INFRAESTRUTURA (Deploy)
11. CONFIGURAÇÕES DE AMBIENTE
Arquivo	Status
.env (Frontend)	⚠️ Parcial
.env (Backend)	⚠️ Parcial
.env.example	❌ Não existe
12. PIPELINE CI/CD
Item	Status
Build script	✅ Existe
Type check	✅ Existe
Lint	❌ Não configurado
Testes	❌ Não existe
13. WEBSOCKET (Tempo Real)
Item	Status
Socket.io	❌ Não configurado
Real-time updates	❌ Não existe
---
📊 MATRIZ DE IMPACTO
FASE 1 - FOUNDATION (Semanas 1-2)
├── Auth Context + Protected Routes
├── API Service Layer
├── Conectar Login/Signup ao backend
└── Estado inicial do usuário
FASE 2 - CORE FEATURES (Semanas 3-4)
├── Dashboard → API (Attention Metrics)
├── Profile → API (User Data)
├── Wallet Connect → Phantom SDK
└── Backend → Database (PostgreSQL)
FASE 3 - POLISH (Semanas 5-6)
├── Error handling + Loading states
├── Form validation (Zod)
├── Toast notifications
└── CI/CD pipeline
FASE 4 - ADVANCED (Semanas 7-8)
├── WebSocket for real-time
├── Support Chat real
├── Export/Import data
└── Mobile app (se aplicável)
---
❓ PERGUNTAS CLARIFICADORAS
1. Banco de dados: Qual você prefere? (PostgreSQL, MySQL, MongoDB)
2. State management: Prefere Context API simples ou quer algo mais robusto como Zustand/React Query?
3. Wallet Integration: Phantom e WalletConnect são para qual blockchain? (Solana, Ethereum, Multi-chain?)
4. Backend location: O backend está no mesmo repositório mas não está rodando. Onde você quer que ele seja executado? (Local, Docker, Cloud)