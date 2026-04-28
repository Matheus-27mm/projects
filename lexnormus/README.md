# LexNormus

> **PT-BR** | [English below](#english)

---

## 🇧🇷 Português

### Sobre

LexNormus é uma plataforma SaaS de gestão jurídica com assistente de inteligência artificial integrado. Desenvolvida para advogados autônomos e escritórios de advocacia de qualquer porte, centraliza o gerenciamento de processos, clientes, documentos, financeiro e comunicação em um único sistema acessível via navegador.

O sistema é composto por um frontend React servido via Vercel, um backend FastAPI hospedado no Render, banco de dados PostgreSQL via Supabase e integrações com DataJud/CNJ, Asaas (pagamentos) e Resend (emails).

---

### Funcionalidades

#### NORMA IA
Assistente jurídica com contexto real do escritório. Responde perguntas sobre processos, clientes e legislação em linguagem natural. Integrada ao DataJud para consulta de movimentações em tempo real e ao gerador de petições para produzir documentos a partir dos dados do processo. Nos planos superiores, a NORMA aprende com os documentos do próprio escritório via RAG.

#### Gerador de Petições
Mais de 100 tipos de documentos jurídicos com templates validados — do peticionamento inicial ao recursal. O usuário descreve o que precisa em linguagem livre e o sistema gera uma minuta estruturada e editável.

#### Integração DataJud / CNJ
Consulta automática ao sistema DataJud do CNJ. Ao informar o número do processo, o sistema retorna movimentações, partes, documentos e prazos sem que o usuário precise acessar o portal do tribunal.

#### RAG Documental
O advogado faz upload de contratos, petições ou processos em PDF. O sistema processa o documento, gera embeddings via pgvector e permite que a NORMA responda perguntas específicas sobre o conteúdo — cláusulas, partes, datas, obrigações.

#### RBAC — Controle de Acesso por Perfil
Quatro níveis de acesso configuráveis por escritório: `admin`, `advogado`, `secretaria` e `estagiário`. Cada perfil acessa apenas os módulos e dados pertinentes à sua função.

#### Painel Financeiro
Gestão de honorários, repasses e inadimplência com alertas automáticos de vencimento. Visualização consolidada por cliente, processo e período.

#### Gestão de Processos e Clientes
Cadastro completo de clientes com histórico de atendimentos. Vinculação de processos, documentos e comunicações por cliente. Timeline cronológica de negociações e movimentações.

#### Autenticação e Segurança
- Login com email/senha ou conta Google (OAuth)
- Autenticação em dois fatores via TOTP (Google Authenticator, Authy)
- JWT com refresh token automático
- Senhas armazenadas com bcrypt
- Rate limiting em todas as rotas sensíveis
- Headers de segurança via Helmet
- Content Security Policy (CSP) configurada
- Queries parametrizadas — sem risco de SQL injection

#### Painel SuperAdmin
Visão consolidada de todos os escritórios cadastrados. Alertas de trial vencido, bloqueio manual de contas, métricas de MRR e uso.

#### Trial Gratuito
7 dias de acesso completo sem necessidade de cartão de crédito. Ao final do trial, o sistema bloqueia o acesso até que a assinatura seja ativada via Asaas (PIX, boleto ou cartão).

---

### Stack

#### Frontend
| Tecnologia | Uso |
|---|---|
| React 18 | Framework principal |
| Vite | Bundler e dev server |
| React Router DOM | Roteamento client-side |
| Lucide React | Ícones |
| Vercel | Hospedagem e CI/CD |

#### Backend
| Tecnologia | Uso |
|---|---|
| Python 3.11 | Linguagem principal |
| FastAPI | Framework da API REST |
| SQLAlchemy | ORM |
| Alembic | Migrações de banco |
| LangChain | Orquestração de IA |
| Anthropic Claude | Modelo de linguagem (NORMA) |
| pgvector | Busca semântica (RAG) |
| Render | Hospedagem e CI/CD |

#### Serviços externos
| Serviço | Uso |
|---|---|
| Supabase | PostgreSQL + Auth + Storage |
| Asaas | Pagamentos (PIX, boleto, cartão) |
| Resend | Emails transacionais |
| DataJud / CNJ | Consulta processual |

---

### Estrutura do projeto

```
lexelite-app/
├── frontend/
│   ├── public/
│   │   └── videos/              # Vídeos de fundo (login, landing)
│   └── src/
│       ├── pages/               # Páginas da aplicação
│       │   ├── landing.jsx      # Página pública
│       │   ├── login.jsx        # Login, cadastro, MFA, recuperação
│       │   ├── dashboard.jsx    # Painel principal
│       │   ├── clientes.jsx     # Gestão de clientes
│       │   ├── documentos.jsx   # Gestão de documentos
│       │   ├── financeiro.jsx   # Painel financeiro
│       │   ├── gerador-peticoes.jsx
│       │   ├── consulta-tribunais.jsx
│       │   ├── configuracoes.jsx
│       │   ├── painel-admin.jsx # SuperAdmin
│       │   └── portal-*.jsx     # Portal do credor
│       ├── services/
│       │   └── supabase.js      # Cliente Supabase configurado
│       └── assets/
│           └── videos/          # Assets de vídeo (dev)
│
└── backend/
    ├── main.py                  # Entrypoint FastAPI
    ├── requirements.txt
    ├── app/
    │   ├── routers/             # Rotas da API
    │   │   ├── auth.py
    │   │   ├── usuarios.py
    │   │   ├── processos.py
    │   │   ├── clientes.py
    │   │   ├── financeiro.py
    │   │   ├── peticoes.py
    │   │   ├── rag.py           # Upload e query de documentos
    │   │   └── email.py
    │   ├── models/              # Modelos SQLAlchemy
    │   ├── schemas/             # Schemas Pydantic
    │   ├── ai/
    │   │   ├── norma.py         # Lógica da NORMA IA
    │   │   └── document_analyzer.py
    │   └── middleware/
    │       └── auth.py          # Validação JWT
    └── migrations/              # Migrações Alembic
        └── versions/
```

---

### Banco de dados

Principais tabelas no Supabase (PostgreSQL):

```sql
escritorios       -- Tenants do sistema (multi-tenant)
usuarios          -- Usuários vinculados a escritórios (RBAC)
processos         -- Processos jurídicos
clientes          -- Clientes dos escritórios
documentos        -- Documentos vinculados a processos
financeiro        -- Lançamentos financeiros
rag_documentos    -- Documentos indexados para RAG
rag_chunks        -- Chunks com embeddings (pgvector)
assinaturas       -- Controle de planos e trial
```

Extensão necessária:
```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

---

### Variáveis de ambiente

**Frontend** — `frontend/.env`
```env
VITE_SUPABASE_URL=https://seu-projeto.supabase.co
VITE_SUPABASE_ANON_KEY=sua-anon-key
VITE_API_URL=https://api.lexnormus.com.br
VITE_SUPERADMIN_EMAIL=seu@email.com
```

**Backend** — `backend/.env`
```env
DATABASE_URL=postgresql://user:password@host:5432/db
SUPABASE_URL=https://seu-projeto.supabase.co
SUPABASE_SERVICE_KEY=sua-service-key
ANTHROPIC_API_KEY=sk-ant-...
ASAAS_API_KEY=sua-chave-asaas
RESEND_API_KEY=re_...
SUPERADMIN_SECRET=segredo-superadmin
JWT_SECRET=segredo-jwt
```

---

### Como rodar localmente

#### Pré-requisitos
- Node.js 18+
- Python 3.11+
- Conta no Supabase com projeto configurado
- Variáveis de ambiente preenchidas

#### Frontend
```bash
cd frontend
npm install
npm run dev
# Acesse: http://localhost:5173
```

#### Backend
```bash
cd backend

# Criar ambiente virtual
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

# Instalar dependências
pip install -r requirements.txt

# Rodar migrações
alembic upgrade head

# Iniciar servidor
uvicorn main:app --reload --port 8000
# Documentação: http://localhost:8000/docs
```

#### Rodar tudo junto (opcional)
```bash
# Na raiz do projeto
npm install concurrently --save-dev
npm run dev
# Requer script "dev" no package.json raiz com concurrently
```

---

### Deploy

#### Frontend — Vercel
1. Conecta o repositório GitHub ao Vercel
2. Define as variáveis de ambiente no painel do Vercel
3. Qualquer push para `main` dispara deploy automático

#### Backend — Render
1. Cria um Web Service no Render apontando para a pasta `backend/`
2. Define as variáveis de ambiente no painel do Render
3. Comando de start: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. Qualquer push para `main` dispara deploy automático

#### Banco — Supabase
- Migrações são aplicadas manualmente via SQL Editor do Supabase
- Arquivos de migração ficam em `backend/migrations/versions/`

---

### Autenticação

O sistema utiliza Supabase Auth como provedor principal:

- **Email/senha** — com hash bcrypt, validação de força de senha no frontend
- **Google OAuth** — configurado via Google Cloud Console + Supabase Auth Providers
- **2FA TOTP** — suporte a Google Authenticator e Authy, verificação via `supabase.auth.mfa`
- **JWT** — tokens com expiração curta e refresh automático
- **Rate limiting** — proteção contra brute force nas rotas de autenticação

Para configurar o OAuth Google:
1. Criar projeto no Google Cloud Console
2. Configurar a tela de consentimento OAuth
3. Gerar Client ID e Client Secret
4. Registrar a Callback URL do Supabase nas URIs autorizadas
5. Inserir as credenciais no painel do Supabase → Authentication → Providers → Google

---

### Rotas da API

| Método | Rota | Descrição |
|---|---|---|
| POST | `/api/auth/login` | Login com email/senha |
| POST | `/api/auth/refresh` | Refresh do JWT |
| GET | `/api/usuarios/me` | Dados do usuário autenticado |
| GET | `/api/processos` | Lista processos do escritório |
| POST | `/api/processos` | Cria novo processo |
| GET | `/api/clientes` | Lista clientes |
| POST | `/api/peticoes/gerar` | Gera petição via IA |
| POST | `/api/rag/upload` | Indexa documento para RAG |
| POST | `/api/rag/query` | Consulta semântica em documento |
| GET | `/api/rag/documentos/{id}` | Lista documentos indexados |
| POST | `/api/email/boas-vindas` | Envia email de boas-vindas |
| GET | `/api/admin/escritorios` | Lista escritórios (SuperAdmin) |

Documentação interativa disponível em `/docs` (Swagger UI) quando o backend está rodando.

---

### Segurança

- Todas as rotas protegidas exigem JWT válido no header `Authorization: Bearer <token>`
- Permissões verificadas por escritório — um usuário nunca acessa dados de outro tenant
- Uploads de arquivo validados por tipo e tamanho antes do processamento
- Logs de auditoria para ações críticas (criação, edição e exclusão de registros sensíveis)
- Variáveis de ambiente nunca expostas no frontend — apenas as prefixadas com `VITE_`

---

### Licença

Uso privado. Todos os direitos reservados.

---

---

<a name="english"></a>

## 🇺🇸 English

### About

LexNormus is a legal management SaaS platform with an integrated AI assistant. Built for solo lawyers and law firms of any size, it centralizes case management, clients, documents, financials, and communication in a single browser-based system.

The system consists of a React frontend served via Vercel, a FastAPI backend hosted on Render, a PostgreSQL database via Supabase, and integrations with DataJud/CNJ (Brazilian court API), Asaas (payments), and Resend (transactional emails).

---

### Features

#### NORMA AI
Legal assistant with real firm context. Answers questions about cases, clients, and legislation in natural language. Integrated with DataJud for real-time case lookup and with the petition generator to produce documents from case data. On higher plans, NORMA learns from the firm's own documents via RAG.

#### Petition Generator
Over 100 legal document types with validated templates — from initial filings to appeals. The user describes what they need in plain language and the system generates a structured, editable draft.

#### DataJud / CNJ Integration
Automatic lookup on the Brazilian court system (DataJud/CNJ). Given a case number, the system returns movements, parties, documents, and deadlines without the user needing to access the court portal directly.

#### Document RAG
Lawyers upload contracts, petitions, or case files in PDF format. The system processes the document, generates embeddings via pgvector, and allows NORMA to answer specific questions about the content — clauses, parties, dates, obligations.

#### RBAC — Role-Based Access Control
Four configurable access levels per firm: `admin`, `lawyer`, `secretary`, and `intern`. Each role accesses only the modules and data relevant to their function.

#### Financial Dashboard
Fee management, payouts, and overdue tracking with automatic due date alerts. Consolidated view by client, case, and period.

#### Case and Client Management
Full client registration with service history. Cases, documents, and communications linked per client. Chronological timeline of negotiations and case movements.

#### Authentication and Security
- Login with email/password or Google account (OAuth)
- Two-factor authentication via TOTP (Google Authenticator, Authy)
- JWT with automatic refresh token
- Passwords stored with bcrypt
- Rate limiting on all sensitive routes
- Security headers via Helmet
- Content Security Policy (CSP) configured
- Parameterized queries — no SQL injection risk

#### SuperAdmin Panel
Consolidated view of all registered firms. Expired trial alerts, manual account blocking, MRR and usage metrics.

#### Free Trial
7 days of full access with no credit card required. After the trial ends, the system blocks access until a subscription is activated via Asaas (PIX, bank slip, or credit card).

---

### Stack

#### Frontend
| Technology | Purpose |
|---|---|
| React 18 | Main framework |
| Vite | Bundler and dev server |
| React Router DOM | Client-side routing |
| Lucide React | Icons |
| Vercel | Hosting and CI/CD |

#### Backend
| Technology | Purpose |
|---|---|
| Python 3.11 | Main language |
| FastAPI | REST API framework |
| SQLAlchemy | ORM |
| Alembic | Database migrations |
| LangChain | AI orchestration |
| Anthropic Claude | Language model (NORMA) |
| pgvector | Semantic search (RAG) |
| Render | Hosting and CI/CD |

#### External Services
| Service | Purpose |
|---|---|
| Supabase | PostgreSQL + Auth + Storage |
| Asaas | Payments (PIX, bank slip, credit card) |
| Resend | Transactional emails |
| DataJud / CNJ | Brazilian court case lookup |

---

### Project Structure

```
lexelite-app/
├── frontend/
│   ├── public/
│   │   └── videos/              # Background videos (login, landing)
│   └── src/
│       ├── pages/               # Application pages
│       │   ├── landing.jsx      # Public landing page
│       │   ├── login.jsx        # Login, signup, MFA, password recovery
│       │   ├── dashboard.jsx    # Main dashboard
│       │   ├── clientes.jsx     # Client management
│       │   ├── documentos.jsx   # Document management
│       │   ├── financeiro.jsx   # Financial dashboard
│       │   ├── gerador-peticoes.jsx
│       │   ├── consulta-tribunais.jsx
│       │   ├── configuracoes.jsx
│       │   ├── painel-admin.jsx # SuperAdmin panel
│       │   └── portal-*.jsx     # Creditor portal
│       ├── services/
│       │   └── supabase.js      # Configured Supabase client
│       └── assets/
│           └── videos/          # Video assets (dev)
│
└── backend/
    ├── main.py                  # FastAPI entrypoint
    ├── requirements.txt
    ├── app/
    │   ├── routers/             # API routes
    │   │   ├── auth.py
    │   │   ├── usuarios.py
    │   │   ├── processos.py
    │   │   ├── clientes.py
    │   │   ├── financeiro.py
    │   │   ├── peticoes.py
    │   │   ├── rag.py           # Document upload and query
    │   │   └── email.py
    │   ├── models/              # SQLAlchemy models
    │   ├── schemas/             # Pydantic schemas
    │   ├── ai/
    │   │   ├── norma.py         # NORMA AI logic
    │   │   └── document_analyzer.py
    │   └── middleware/
    │       └── auth.py          # JWT validation
    └── migrations/              # Alembic migrations
        └── versions/
```

---

### Database

Main tables in Supabase (PostgreSQL):

```sql
escritorios       -- System tenants (multi-tenant)
usuarios          -- Users linked to firms (RBAC)
processos         -- Legal cases
clientes          -- Firm clients
documentos        -- Documents linked to cases
financeiro        -- Financial entries
rag_documentos    -- Documents indexed for RAG
rag_chunks        -- Chunks with embeddings (pgvector)
assinaturas       -- Plan and trial control
```

Required extension:
```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

---

### Environment Variables

**Frontend** — `frontend/.env`
```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_URL=https://api.lexnormus.com.br
VITE_SUPERADMIN_EMAIL=your@email.com
```

**Backend** — `backend/.env`
```env
DATABASE_URL=postgresql://user:password@host:5432/db
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your-service-key
ANTHROPIC_API_KEY=sk-ant-...
ASAAS_API_KEY=your-asaas-key
RESEND_API_KEY=re_...
SUPERADMIN_SECRET=superadmin-secret
JWT_SECRET=jwt-secret
```

---

### Running Locally

#### Prerequisites
- Node.js 18+
- Python 3.11+
- Supabase account with configured project
- Environment variables filled in

#### Frontend
```bash
cd frontend
npm install
npm run dev
# Access: http://localhost:5173
```

#### Backend
```bash
cd backend

# Create virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

# Install dependencies
pip install -r requirements.txt

# Run migrations
alembic upgrade head

# Start server
uvicorn main:app --reload --port 8000
# Docs: http://localhost:8000/docs
```

#### Running Both Together (optional)
```bash
# From project root
npm install concurrently --save-dev
npm run dev
# Requires a "dev" script in root package.json using concurrently
```

---

### Deployment

#### Frontend — Vercel
1. Connect the GitHub repository to Vercel
2. Set environment variables in the Vercel dashboard
3. Any push to `main` triggers automatic deployment

#### Backend — Render
1. Create a Web Service on Render pointing to the `backend/` folder
2. Set environment variables in the Render dashboard
3. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. Any push to `main` triggers automatic deployment

#### Database — Supabase
- Migrations are applied manually via the Supabase SQL Editor
- Migration files are located in `backend/migrations/versions/`

---

### Authentication

The system uses Supabase Auth as the main provider:

- **Email/password** — bcrypt hashing, password strength validation on frontend
- **Google OAuth** — configured via Google Cloud Console + Supabase Auth Providers
- **2FA TOTP** — Google Authenticator and Authy support, verified via `supabase.auth.mfa`
- **JWT** — short-lived tokens with automatic refresh
- **Rate limiting** — brute force protection on authentication routes

To configure Google OAuth:
1. Create a project in Google Cloud Console
2. Configure the OAuth consent screen
3. Generate Client ID and Client Secret
4. Register the Supabase Callback URL in the authorized redirect URIs
5. Enter the credentials in Supabase → Authentication → Providers → Google

---

### API Routes

| Method | Route | Description |
|---|---|---|
| POST | `/api/auth/login` | Login with email/password |
| POST | `/api/auth/refresh` | JWT refresh |
| GET | `/api/usuarios/me` | Authenticated user data |
| GET | `/api/processos` | List firm cases |
| POST | `/api/processos` | Create new case |
| GET | `/api/clientes` | List clients |
| POST | `/api/peticoes/gerar` | Generate petition via AI |
| POST | `/api/rag/upload` | Index document for RAG |
| POST | `/api/rag/query` | Semantic query on document |
| GET | `/api/rag/documentos/{id}` | List indexed documents |
| POST | `/api/email/boas-vindas` | Send welcome email |
| GET | `/api/admin/escritorios` | List firms (SuperAdmin) |

Interactive documentation available at `/docs` (Swagger UI) when the backend is running.

---

### Security

- All protected routes require a valid JWT in the `Authorization: Bearer <token>` header
- Permissions are verified per firm — a user never accesses another tenant's data
- File uploads are validated by type and size before processing
- Audit logs for critical actions (creation, editing, and deletion of sensitive records)
- Environment variables never exposed in the frontend — only those prefixed with `VITE_`

---

### License

Private use. All rights reserved.
