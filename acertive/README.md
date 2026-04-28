<div align="center">

# ACERTIVE

### Sistema de Gestão e Cobrança Inteligente
### *Intelligent Debt Collection & Management System*

[![Status](https://img.shields.io/badge/status-em%20produção-success?style=flat-square)](https://github.com/Matheus-27mm/acertive-system)
[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Security](https://img.shields.io/badge/pentest-80%2F100-blue?style=flat-square&logo=shield&logoColor=white)](#-security)
[![License](https://img.shields.io/badge/license-Proprietary-lightgrey?style=flat-square)](#-license)

**Plataforma SaaS completa para empresas de assessoria de cobrança**, com chatbot WhatsApp, gestão de acordos, portal exclusivo para credores e auditoria de segurança nível enterprise.

</div>

---

## 🌎 Languages / Idiomas

- 🇧🇷 [Português (BR)](#-português-br)
- 🇺🇸 [English](#-english)

---

# 🇧🇷 Português (BR)

## 📋 Sobre o Projeto

ACERTIVE é um sistema completo de gestão e cobrança desenvolvido sob medida para empresas de assessoria que atendem múltiplos credores. O sistema centraliza fluxos de cobrança, automação via WhatsApp, geração de acordos com PIX, e fornece um portal exclusivo onde credores acompanham KPIs em tempo real.

Atualmente em produção, atendendo carteira ativa com integração Meta WhatsApp Business e APIs financeiras (Asaas).

## ✨ Funcionalidades Principais

### Gestão de Carteira
- Importação em lote de cobranças via planilha (xlsx/csv)
- Cadastro multi-credor com regras de juros/multa por contrato
- Cálculo automático de valor atualizado (multa fixa + juros progressivos)
- Categorização de devedores por aging (vencido, em atraso, jurídico, incobrável)

### Automação WhatsApp (Suri/Meta Business API)
- Disparo de templates aprovados pela Meta, segmentados por credor
- Chatbot com fluxo `1/2/3` interativo: detalhes da pendência, negociação, alegação de pagamento
- Submenu de negociação (à vista, parcelar, atendente humano)
- Salvaguarda automática: não dispara relembrete pra quem respondeu nos últimos 7 dias
- Disparo em massa com filtros por aging, credor e canal

### Acordos & Pagamentos
- Criação de acordos parcelados com geração automática de PIX (via Asaas)
- Webhook de confirmação de pagamento atualiza status em tempo real
- Lembretes automáticos de parcelas a vencer
- Histórico de negociações com timeline completa por devedor

### Portal do Credor
- Endpoint isolado (`/portal.html`) com autenticação JWT independente
- Dashboard com KPIs próprios: aging, acordos ativos, valor recuperado
- Exportação Excel/PDF de relatórios
- Gestão de usuários por credor (admin pode adicionar funcionários)

### Auditoria & Compliance
- AuditLog global: todas as alterações relevantes registradas com diff antes/depois
- Histórico de acionamentos por devedor (canal, operador, resultado)
- Exportação de relatórios para auditoria externa

## 🛠️ Stack Técnica

| Camada | Tecnologia |
|--------|-----------|
| **Backend** | Node.js, Express, JWT, bcrypt |
| **Banco de Dados** | PostgreSQL (Render-hosted) |
| **Frontend** | HTML5, CSS3, JavaScript Vanilla, SPA pattern |
| **Integrações** | Meta WhatsApp Business API (via Suri), Asaas (PIX/Boleto), Nodemailer (SMTP) |
| **Deploy** | Render (auto-deploy via GitHub push) |
| **Segurança** | Helmet, CSP, express-rate-limit, parameterized queries |

## 🔒 Security

A segurança é tratada como requisito de primeira classe neste projeto. Práticas implementadas:

### Autenticação & Autorização
- **JWT** com tokens assinados HMAC-SHA256 e secret forte (256 bits, gerado aleatoriamente)
- **bcrypt** com salt rounds = 10 para hashing de senhas
- **RBAC** (Role-Based Access Control): perfis `admin` e `operador` com permissões granulares
- **Tokens isolados** por contexto: usuários internos, credores, e clientes finais usam JWTs distintos

### Proteção contra Injeção
- **100% das queries** usam parâmetros (`$1, $2, ...`) — zero concatenação de strings com input do usuário
- **CSP (Content Security Policy)** configurada via Helmet bloqueando inline scripts não autorizados
- **Sanitização de uploads**: validação de tipo MIME e tamanho antes do parsing

### Rate Limiting & Anti-Bruteforce
- `express-rate-limit` em endpoints sensíveis (login, redefinição de senha)
- Lockout temporário após N tentativas falhas
- Identificação por IP real (configuração `trust proxy` para ambiente Render)

### Gestão de Credenciais
- **Zero credentials hardcoded em código** (validado por auditoria com Git history scan)
- Todas as variáveis sensíveis em `process.env.*` (configuradas no painel Render)
- `.env.example` versionado documentando variáveis necessárias (sem valores)
- `.gitignore` protegendo `.env`, logs, backups e arquivos de auditoria

### Auditoria Contínua
- **Pentest interno**: score 80/100 (vulnerabilidades remediadas: CSP, headers, rate limiting)
- **GitHub Secret Scanning** ativo no repositório
- Audit log de todas as ações administrativas com timestamp + ator + payload diff
- Roadmap inclui pre-commit hooks com Gitleaks para defesa contínua

## 🚀 Setup Local

### Pré-requisitos
- Node.js 18+
- PostgreSQL 15+
- Conta na Suri/Chatbot Maker (para WhatsApp)
- Conta no Asaas (sandbox ou produção, opcional)

### Instalação

```bash
# 1. Clonar o repositório
git clone https://github.com/Matheus-27mm/acertive-system.git
cd acertive-system/backend

# 2. Instalar dependências
npm install

# 3. Configurar variáveis de ambiente
cp .env.example .env
# Editar .env com seus valores
```

### Variáveis de Ambiente Obrigatórias

```env
# Banco de dados
DATABASE_URL=postgres://user:password@host:5432/dbname

# Autenticação
JWT_SECRET=<string-aleatória-64-chars>

# Suri (WhatsApp)
SURI_ENDPOINT=https://cbm-wap-babysuri-XXXX.azurewebsites.net
SURI_TOKEN=<seu-token>
SURI_CHATBOT_ID=cbXXXXXXXX
SURI_CHANNEL_ID=wpXXXXXXXX
SURI_TEMPLATE_DEFAULT=XXXXXXXXX

# Email (opcional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=seu@email.com
SMTP_PASS=<senha-de-app>
EMAIL_FROM=ACERTIVE <seu@email.com>

# Asaas (opcional)
ASAAS_API_KEY=<sua-key>
ASAAS_BASE_URL=https://sandbox.asaas.com/api/v3
```

### Migrações do Banco

```bash
# Aplicar schema inicial
psql $DATABASE_URL < schema/init.sql

# Aplicar coluna chave_template em credores (necessário pra v3.5+)
psql $DATABASE_URL -c "ALTER TABLE credores ADD COLUMN IF NOT EXISTS chave_template VARCHAR(50);"
```

### Executar

```bash
# Desenvolvimento (com hot reload)
npm run dev

# Produção
npm start
```

Servidor sobe em `http://localhost:3000` por padrão.

## 📦 Deploy

Deploy automatizado via Render:

1. Conectar repositório GitHub no Render Dashboard
2. Criar Web Service apontando pra pasta `backend/`
3. Configurar todas as variáveis de ambiente no painel Environment
4. Push em `main` dispara deploy automático

## 🗺️ Roadmap

- [x] v3.7 — Centralização de constantes de atendimento
- [x] v3.6 — Migração de credenciais para env vars
- [x] v3.5 — Templates segmentados por credor (Transbyshop em produção)
- [ ] v4.0 — Pre-commit hooks com Gitleaks (defesa contínua)
- [ ] v4.1 — LGPD compliance fase 1 (audit log público, mascaramento CPF)
- [ ] v4.2 — Cron jobs de notificação automática
- [ ] v4.3 — Integração Fogás e Odonto Vitta (templates próprios)
- [ ] v5.0 — Refatoração modular (server.js → microserviços lógicos)

## 👨‍💻 Autor

**Matheus** — Engenheiro de Computação e desenvolvedor full-stack focado em AppSec, fundador da [Inceptum](https://github.com/inceptumcorp).

- 📍 Manaus, AM, Brasil
- 🛡️ Foco: Application Security & Sistemas Financeiros
- 📚 Estudando: Cisco CyberOps, Black Hat Python, GrapheneOS

## 📄 License

Software proprietário. Todos os direitos reservados.

---

# 🇺🇸 English

## 📋 About

ACERTIVE is a comprehensive collection management system custom-built for debt collection agencies serving multiple creditors. The platform centralizes collection workflows, WhatsApp automation, PIX-based agreements, and provides a dedicated portal where creditors track real-time KPIs.

Currently in production, serving an active portfolio with Meta WhatsApp Business API integration and financial APIs (Asaas).

## ✨ Key Features

### Portfolio Management
- Bulk import of collections via spreadsheet (xlsx/csv)
- Multi-creditor registry with per-contract interest/penalty rules
- Automatic calculation of updated balance (fixed penalty + progressive interest)
- Debtor categorization by aging (overdue, in arrears, legal, write-off)

### WhatsApp Automation (Suri/Meta Business API)
- Meta-approved template dispatch, segmented by creditor
- Interactive `1/2/3` chatbot flow: debt details, negotiation, payment claim
- Negotiation submenu (lump sum, installments, human agent)
- Automatic safeguard: no reminder dispatched to recently-responsive contacts (7-day window)
- Mass dispatch with aging, creditor, and channel filters

### Agreements & Payments
- Installment-based agreements with auto-generated PIX (via Asaas)
- Payment confirmation webhook updates status in real time
- Automatic upcoming-installment reminders
- Full negotiation timeline per debtor

### Creditor Portal
- Isolated endpoint (`/portal.html`) with independent JWT authentication
- KPI-focused dashboard: aging, active agreements, recovered amount
- Excel/PDF report exports
- Per-creditor user management (admin can add staff)

### Auditing & Compliance
- Global AuditLog: all relevant changes recorded with before/after diff
- Per-debtor activity history (channel, operator, outcome)
- External audit-ready report exports

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Node.js, Express, JWT, bcrypt |
| **Database** | PostgreSQL (Render-hosted) |
| **Frontend** | HTML5, CSS3, Vanilla JavaScript, SPA pattern |
| **Integrations** | Meta WhatsApp Business API (via Suri), Asaas (PIX/Boleto), Nodemailer (SMTP) |
| **Deployment** | Render (auto-deploy via GitHub push) |
| **Security** | Helmet, CSP, express-rate-limit, parameterized queries |

## 🔒 Security

Security is treated as a first-class requirement. Implemented practices:

### Authentication & Authorization
- **JWT** with HMAC-SHA256 signed tokens and strong secret (256 bits, randomly generated)
- **bcrypt** with salt rounds = 10 for password hashing
- **RBAC** (Role-Based Access Control): `admin` and `operator` profiles with granular permissions
- **Context-isolated tokens**: internal users, creditors, and end clients use distinct JWTs

### Injection Protection
- **100% of queries** use parameters (`$1, $2, ...`) — zero string concatenation with user input
- **CSP (Content Security Policy)** configured via Helmet, blocking unauthorized inline scripts
- **Upload sanitization**: MIME type and size validation before parsing

### Rate Limiting & Anti-Bruteforce
- `express-rate-limit` on sensitive endpoints (login, password reset)
- Temporary lockout after N failed attempts
- Real IP identification (`trust proxy` config for Render environment)

### Credentials Management
- **Zero hardcoded credentials in code** (validated via Git history scan audit)
- All sensitive variables in `process.env.*` (configured in Render dashboard)
- `.env.example` versioned to document required variables (no values)
- `.gitignore` protecting `.env`, logs, backups, and audit files

### Continuous Auditing
- **Internal pentest**: 80/100 score (remediated vulnerabilities: CSP, headers, rate limiting)
- **GitHub Secret Scanning** active on repository
- Audit log for all administrative actions with timestamp + actor + payload diff
- Roadmap includes Gitleaks pre-commit hooks for continuous defense

## 🚀 Local Setup

### Prerequisites
- Node.js 18+
- PostgreSQL 15+
- Suri/Chatbot Maker account (for WhatsApp)
- Asaas account (sandbox or production, optional)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Matheus-27mm/acertive-system.git
cd acertive-system/backend

# 2. Install dependencies
npm install

# 3. Configure environment variables
cp .env.example .env
# Edit .env with your values
```

### Required Environment Variables

```env
# Database
DATABASE_URL=postgres://user:password@host:5432/dbname

# Authentication
JWT_SECRET=<random-64-char-string>

# Suri (WhatsApp)
SURI_ENDPOINT=https://cbm-wap-babysuri-XXXX.azurewebsites.net
SURI_TOKEN=<your-token>
SURI_CHATBOT_ID=cbXXXXXXXX
SURI_CHANNEL_ID=wpXXXXXXXX
SURI_TEMPLATE_DEFAULT=XXXXXXXXX

# Email (optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your@email.com
SMTP_PASS=<app-password>
EMAIL_FROM=ACERTIVE <your@email.com>

# Asaas (optional)
ASAAS_API_KEY=<your-key>
ASAAS_BASE_URL=https://sandbox.asaas.com/api/v3
```

### Database Migrations

```bash
# Apply initial schema
psql $DATABASE_URL < schema/init.sql

# Apply chave_template column on credores (required for v3.5+)
psql $DATABASE_URL -c "ALTER TABLE credores ADD COLUMN IF NOT EXISTS chave_template VARCHAR(50);"
```

### Run

```bash
# Development (with hot reload)
npm run dev

# Production
npm start
```

Server starts on `http://localhost:3000` by default.

## 📦 Deployment

Automated deployment via Render:

1. Connect GitHub repository in Render Dashboard
2. Create Web Service pointing to `backend/` folder
3. Configure all environment variables in Environment panel
4. Pushing to `main` triggers automatic deployment

## 🗺️ Roadmap

- [x] v3.7 — Centralization of attendance constants
- [x] v3.6 — Credentials migration to env vars
- [x] v3.5 — Creditor-segmented templates (Transbyshop in production)
- [ ] v4.0 — Pre-commit hooks with Gitleaks (continuous defense)
- [ ] v4.1 — LGPD (Brazilian GDPR) compliance phase 1 (public audit log, CPF masking)
- [ ] v4.2 — Automatic notification cron jobs
- [ ] v4.3 — Fogás and Odonto Vitta integration (own templates)
- [ ] v5.0 — Modular refactoring (server.js → logical microservices)

## 👨‍💻 Author

**Matheus** — Computer Engineering student and full-stack developer focused on AppSec, co-founder of [Inceptum](https://github.com/inceptumcorp).

- 📍 Manaus, AM, Brazil
- 🛡️ Focus: Application Security & Financial Systems
- 📚 Studying: Cisco CyberOps, Black Hat Python, GrapheneOS

## 📄 License

Proprietary software. All rights reserved.

---

<div align="center">

**Built with care in Manaus, Brazil 🌳**

</div>
