<div align="center">

# 🌳 SELA

### Plataforma de Compliance EUDR e Rastreabilidade para Cooperativas Amazônicas

*Sistema de rastreabilidade geoespacial que ajuda cooperativas agrícolas amazônicas a comprovarem a origem livre de desmatamento de seus produtos, em conformidade com o Regulamento Europeu Anti-Desmatamento (EUDR / Regulamento UE 2023/1115).*

[![Status](https://img.shields.io/badge/status-MVP-22c55e?style=flat-square)]()
[![Licen%C3%A7a](https://img.shields.io/badge/licen%C3%A7a-Propriet%C3%A1ria-red?style=flat-square)]()
[![EUDR](https://img.shields.io/badge/EUDR-2023%2F1115-d97706?style=flat-square)]()
[![Stack](https://img.shields.io/badge/stack-React%20%7C%20Node%20%7C%20PostgreSQL-60a5fa?style=flat-square)]()

[English 🇺🇸](./README.md) · [Demo](https://sela-client-inky.vercel.app) · [Issues](https://github.com/Matheus-27mm/sela/issues)

</div>

---

## Visão Geral

A partir do final de 2025, a União Europeia passou a exigir que importadores de commodities como cacau, café, soja, borracha, óleo de palma e madeira comprovassem a origem livre de desmatamento dos seus produtos, por meio do **Regulamento Europeu Anti-Desmatamento (EUDR)**. Para cooperativas amazônicas que operam sem infraestrutura digital, atender a essa exigência se tornou uma barreira significativa de acesso ao mercado europeu.

O **SELA** resolve esse problema oferecendo uma plataforma completa de rastreabilidade e compliance pensada para a realidade de cooperativas amazônicas de pequeno e médio porte — do produtor individual no interior ao consumidor final que escaneia um QR code no produto.

## Screenshots

<table>
  <tr>
    <td width="50%" align="center">
      <strong>Dashboard — Compliance Overview</strong><br>
      <em>EUDR compliance KPIs, ESG score and active alerts at a glance</em><br><br>
      <img src="screenshots/01-dashboard.png" alt="SELA Dashboard with EUDR compliance KPIs, ESG score and environmental alerts" />
    </td>
    <td width="50%" align="center">
      <strong>Geospatial Map — Overview</strong><br>
      <em>All registered farms colored by EUDR compliance status</em><br><br>
      <img src="screenshots/02-map-overview.png" alt="Satellite map showing all registered farms colored by EUDR status" />
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <strong>DETER Alerts Overlay</strong><br>
      <em>Real-time deforestation polygons from INPE crossed with farm boundaries</em><br><br>
      <img src="screenshots/03-map-deter.png" alt="Map with DETER deforestation alerts overlay on farm polygons" />
    </td>
    <td width="50%" align="center">
      <strong>Farm Management</strong><br>
      <em>Detailed view with ESG score, polygon area and risk indicators</em><br><br>
      <img src="screenshots/04-farms.png" alt="Farm management screen with ESG score and territorial data" />
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <strong>Lot Tracking</strong><br>
      <em>Status flow from planting to delivery with traceability codes</em><br><br>
      <img src="screenshots/05-lots.png" alt="Lot management with tracking codes and status flow" />
    </td>
    <td width="50%" align="center">
      <strong>Public Traceability</strong><br>
      <em>QR-accessible page showing the full origin chain to the final consumer</em><br><br>
      <img src="screenshots/06-public-tracking.png" alt="Public traceability page accessible via QR code, showing origin chain" />
    </td>
  </tr>
</table>

## Funcionalidades

### 🛰️ Monitoramento Geoespacial em Tempo Real
- Integração com **DETER/INPE (Terrabrasilis)** para alertas de desmatamento em tempo real
- Cruzamento automático entre polígonos de fazenda e polígonos de desmatamento
- Buffer de risco de 5km em torno de cada propriedade cadastrada
- Imagens de satélite Mapbox com visualização de uso do solo em alta resolução

### 📊 Sistema de Score ESG
- Pontuação tridimensional: **Ambiental** (40%), **Documental** (30%), **Operacional** (30%)
- Classificação de A a E por fazenda
- Cálculo automatizado baseado em documentos, alertas e conformidade operacional

### 📅 Cut-off Date Engine
- Validação automática da data de corte EUDR (após 31 de dezembro de 2020)
- Detecção de mudanças de uso do solo após o limite regulatório
- Status: CONFORME / NÃO CONFORME, com evidência datada

### 🔗 Rastreabilidade Ponta a Ponta
- Cadeia completa: **Produtor → Fazenda → Lote → Consumidor Final**
- Rastreio público via QR code (sem login)
- Fluxo de status: PLANTIO → COLHEITA → TRANSPORTE → ENTREGA
- Histórico imutável de status (pronto para auditoria)

### 📄 Relatórios de Due Diligence
- Geração automatizada de relatórios EUDR em PDF
- Assinatura digital e timestamping
- Pronto para submissão a auditores e importadores europeus

### 🔐 Controle de Acesso por Função (RBAC)
- **ADMIN** — Acesso total ao sistema, audit log
- **COOPERATIVA** — Gerencia produtores, fazendas e lotes da cooperativa
- **PRODUTOR** — Acesso de leitura aos próprios dados de fazenda e lotes
- **AUDITOR** — Acesso de leitura para fins de verificação

### 🔍 Audit Log
- Audit log global com diff antes/depois em todas as operações
- Rastreabilidade total de quem alterou o quê e quando
- Conformidade com requisitos de integridade de dados em auditorias regulatórias

## Stack Técnica

| Camada | Tecnologia |
|--------|------------|
| **Frontend** | React 18, Vite, Mapbox GL JS, Leaflet, React Router |
| **Backend** | Node.js, Express, Prisma ORM |
| **Banco de Dados** | PostgreSQL (Render) |
| **Autenticação** | JWT, bcrypt, Google OAuth |
| **Mapas e Geoespacial** | Mapbox API, OpenStreetMap (Nominatim), Turf.js |
| **APIs Externas** | INPE/DETER (Terrabrasilis), SICAR, MapBiomas |
| **Deploy** | Vercel (frontend), Render (backend + banco) |
| **Relatórios** | PDFKit |

## Arquitetura

```
┌──────────────────────────────────────────────────────────────┐
│                      CLIENT (React)                          │
│  Dashboard · Produtores · Fazendas · Lotes · Mapa · Reports  │
└────────────────────────────────────────┬─────────────────────┘
                                         │ REST API + JWT
┌────────────────────────────────────────▼─────────────────────┐
│                   SERVER (Express + Prisma)                  │
│  Auth · RBAC · ESG · Cut-off · Due Diligence · Audit Log     │
└────────────────┬──────────────────────┬──────────────────────┘
                 │                      │
        ┌────────▼─────────┐   ┌────────▼──────────┐
        │   PostgreSQL     │   │  APIs Externas    │
        │   (Render)       │   │  DETER · INPE     │
        │                  │   │  Mapbox · SICAR   │
        └──────────────────┘   └───────────────────┘
```

## Como Começar

### Pré-requisitos

- Node.js 18+ e npm
- PostgreSQL 15+ (ou serviço gerenciado como Render/Supabase)
- Conta Mapbox (free tier) para o token de acesso

### Setup Local

**1. Clone o repositório**

```bash
git clone https://github.com/Matheus-27mm/sela.git
cd sela
```

**2. Setup do backend**

```bash
cd server
npm install
cp .env.example .env
# Edite o .env com seu DATABASE_URL, JWT_SECRET, etc.
npx prisma generate
npx prisma db push
npx prisma db seed   # cria usuários demo (opcional)
npm run dev
```

O backend vai rodar em `http://localhost:3001`.

**3. Setup do frontend**

```bash
cd ../client
npm install
cp .env.example .env
# Edite o .env com VITE_API_URL e VITE_MAPBOX_TOKEN
npm run dev
```

O frontend vai rodar em `http://localhost:5178`.

### Variáveis de Ambiente

**Backend (`server/.env`):**

```env
DATABASE_URL=postgresql://user:password@host:5432/sela
JWT_SECRET=<gere com: openssl rand -hex 64>
ALLOWED_ORIGINS=http://localhost:5178,https://seu-frontend.vercel.app
PORT=3001
```

**Frontend (`client/.env`):**

```env
VITE_API_URL=http://localhost:3001
VITE_MAPBOX_TOKEN=pk.seu_token_publico_mapbox
```

> ⚠️ **Nunca commite arquivos `.env`.** Eles estão no `.gitignore` por um motivo.

## Roadmap

### ✅ Fase 1 — MVP (Concluída)
- [x] CRUD de produtores, fazendas e lotes
- [x] Rastreabilidade pública via QR code
- [x] Integração DETER/INPE com alertas em tempo real
- [x] Engine de Score ESG
- [x] Validação de cut-off date
- [x] Relatórios PDF de Due Diligence
- [x] Landing page pública

### 🔵 Fase 2 — Piloto com Cooperativas (Q2 2026)
- [ ] Auto-cadastro de cooperativa com aprovação do admin
- [ ] Sistema de convites para auditores e produtores
- [ ] Acesso do produtor via link único
- [ ] Validação DETER em tempo real no registro de colheita
- [ ] Importação em massa via CSV para onboarding de produtores
- [ ] Suporte multilíngue (PT-BR, EN, ES)

### ⚪ Fase 3 — Escala (Q3-Q4 2026)
- [ ] App mobile (PWA) para técnicos de campo com captura GPS
- [ ] Integração WhatsApp Business para notificações ao produtor
- [ ] Varredura semanal de satélite com resumo por email
- [ ] API pública de verificação de rastreabilidade para importadores
- [ ] Documentação LGPD e templates de DPA

## Estrutura do Projeto

```
sela/
├── client/                      # Frontend React (Vite)
│   ├── public/
│   │   └── landing/             # Landing page pública de marketing
│   ├── src/
│   │   ├── components/          # Sidebar, ExportButton, etc.
│   │   ├── pages/               # Dashboard, Farms, Lots, Map, etc.
│   │   ├── lib/                 # Cliente API, helpers
│   │   └── App.jsx              # Router e layout
│   └── vite.config.js
├── server/                      # Backend Node.js (Express + Prisma)
│   ├── prisma/
│   │   ├── schema.prisma        # Schema do banco
│   │   └── seed.js              # Seed de dados demo
│   ├── services/                # Lógica de negócio
│   │   ├── auditService.js
│   │   ├── cutOffService.js
│   │   ├── deforestationService.js
│   │   ├── dueDiligenceReport.js
│   │   ├── esgScore.js
│   │   └── lotTraceabilityReport.js
│   └── index.js                 # Entry point Express
├── .gitignore
├── package.json
├── README.md                    # Versão em inglês
└── README.pt-BR.md              # Este arquivo
```

## Contribuições

Este projeto está atualmente em desenvolvimento privado. Se você é de uma cooperativa, órgão regulador ou empresa de auditoria interessada em pilotar o SELA, entre em contato pelos canais abaixo.

Para contribuições técnicas:
1. Abra uma issue descrevendo a mudança proposta
2. Aguarde aprovação antes de abrir um PR
3. Siga o estilo de código existente (a base privilegia legibilidade sobre padrões rígidos)

## Licença

Proprietária — Todos os direitos reservados. Para licenciamento, entre em contato com o autor.

## Autor

**Matheus Margarido**
Estudante de Engenharia da Computação · Desenvolvedor full-stack · Aspirante a AppSec
Manaus, Amazonas, Brasil

📧 [Adicionar email profissional]
🔗 [LinkedIn](https://linkedin.com/in/matheusmargarido) · [GitHub](https://github.com/Matheus-27mm)

## Agradecimentos

- **INPE** pelo acesso aberto ao sistema DETER de alertas de desmatamento
- **MapBiomas** pelos dados históricos de uso do solo
- **OCB-AM** e **Cooperar** pelo diálogo contínuo sobre as necessidades das cooperativas amazônicas
- Comunidade open-source por trás do React, Prisma, Mapbox e PostgreSQL

---

<div align="center">

*Construído em Manaus 🌿 com carinho pela Amazônia e por quem a faz prosperar.*

</div>

<div align="center">

# 🌳 SELA

### EUDR Compliance & Traceability Platform for Amazon Cooperatives

*A geospatial traceability system that helps Amazonian agricultural cooperatives prove the deforestation-free origin of their products in compliance with the EU Deforestation Regulation (EUDR / Regulation 2023/1115).*

[![Status](https://img.shields.io/badge/status-MVP-22c55e?style=flat-square)]()
[![License](https://img.shields.io/badge/license-Proprietary-red?style=flat-square)]()
[![EUDR](https://img.shields.io/badge/EUDR-2023%2F1115-d97706?style=flat-square)]()
[![Stack](https://img.shields.io/badge/stack-React%20%7C%20Node%20%7C%20PostgreSQL-60a5fa?style=flat-square)]()

[Português 🇧🇷](./README.pt-BR.md) · [Live Demo](https://sela-client-inky.vercel.app) · [Issues](https://github.com/Matheus-27mm/sela/issues)

</div>

---

## Overview

Starting in late 2025, the European Union began requiring importers of commodities such as cocoa, coffee, soy, rubber, palm oil and timber to prove the deforestation-free origin of their products through the **EU Deforestation Regulation (EUDR)**. For Amazonian cooperatives operating without digital infrastructure, complying with this regulation has become a major barrier to accessing the European market.

**SELA** addresses this challenge by providing an end-to-end traceability and compliance platform tailored to the reality of small and mid-sized Amazonian cooperatives — from individual producers in the rainforest to the consumer scanning a QR code on the final product.
## Screenshots

<table>
  <tr>
    <td width="50%" align="center">
      <strong>Dashboard — Compliance Overview</strong><br>
      <em>EUDR compliance KPIs, ESG score and active alerts at a glance</em><br><br>
      <img src="screenshots/01-dashboard.png" alt="SELA Dashboard with EUDR compliance KPIs, ESG score and environmental alerts" />
    </td>
    <td width="50%" align="center">
      <strong>Geospatial Map — Overview</strong><br>
      <em>All registered farms colored by EUDR compliance status</em><br><br>
      <img src="screenshots/02-map-overview.png" alt="Satellite map showing all registered farms colored by EUDR status" />
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <strong>DETER Alerts Overlay</strong><br>
      <em>Real-time deforestation polygons from INPE crossed with farm boundaries</em><br><br>
      <img src="screenshots/03-map-deter.png" alt="Map with DETER deforestation alerts overlay on farm polygons" />
    </td>
    <td width="50%" align="center">
      <strong>Farm Management</strong><br>
      <em>Detailed view with ESG score, polygon area and risk indicators</em><br><br>
      <img src="screenshots/04-farms.png" alt="Farm management screen with ESG score and territorial data" />
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <strong>Lot Tracking</strong><br>
      <em>Status flow from planting to delivery with traceability codes</em><br><br>
      <img src="screenshots/05-lots.png" alt="Lot management with tracking codes and status flow" />
    </td>
    <td width="50%" align="center">
      <strong>Public Traceability</strong><br>
      <em>QR-accessible page showing the full origin chain to the final consumer</em><br><br>
      <img src="screenshots/06-public-tracking.png" alt="Public traceability page accessible via QR code, showing origin chain" />
    </td>
  </tr>
</table>
## Key Features

### 🛰️ Real-Time Geospatial Monitoring
- Integration with **DETER/INPE (Terrabrasilis)** for real-time deforestation alerts
- Automatic cross-referencing between farm polygons and deforestation polygons
- 5km risk buffer around each registered property
- Mapbox satellite imagery with high-resolution land use visualization

### 📊 ESG Score System
- Three-dimensional scoring: **Environmental** (40%), **Documentary** (30%), **Operational** (30%)
- A–E grade classification per farm
- Automated calculation based on documents, alerts and operational compliance

### 📅 Cut-off Date Engine
- Automatic validation of EUDR cut-off date (post December 31, 2020)
- Detection of land use changes after the regulatory threshold
- Status: COMPLIANT / NON-COMPLIANT with timestamped evidence

### 🔗 End-to-End Traceability
- Full chain: **Producer → Farm → Lot → Final Consumer**
- Public traceability via QR code (no login required)
- Status flow: PLANTING → HARVEST → TRANSPORT → DELIVERY
- Immutable status history (audit-ready)

### 📄 Due Diligence Reports
- Automated generation of EUDR-compliant Due Diligence PDF reports
- Digital signature and timestamping
- Ready for submission to European auditors and importers

### 🔐 Role-Based Access Control (RBAC)
- **ADMIN** — Full system access, audit log
- **COOPERATIVA** — Manages producers, farms, lots within the cooperative
- **PRODUTOR** — Read-only access to own farm and lot data
- **AUDITOR** — Read-only access for verification purposes

### 🔍 Audit Log
- Global audit log with before/after diffs on every operation
- Full traceability of who changed what and when
- Compliance with data integrity requirements for regulatory audits

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React 18, Vite, Mapbox GL JS, Leaflet, React Router |
| **Backend** | Node.js, Express, Prisma ORM |
| **Database** | PostgreSQL (hosted on Render) |
| **Authentication** | JWT, bcrypt, Google OAuth |
| **Maps & Geospatial** | Mapbox API, OpenStreetMap (Nominatim), Turf.js |
| **External APIs** | INPE/DETER (Terrabrasilis), SICAR, MapBiomas |
| **Deployment** | Vercel (frontend), Render (backend + database) |
| **Reports** | PDFKit |

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                         CLIENT (React)                       │
│  Dashboard · Producers · Farms · Lots · Map · Reports        │
└────────────────────────────────────────┬─────────────────────┘
                                         │ REST API + JWT
┌────────────────────────────────────────▼─────────────────────┐
│                      SERVER (Express + Prisma)               │
│  Auth · RBAC · ESG · Cut-off · Due Diligence · Audit Log     │
└────────────────┬──────────────────────┬──────────────────────┘
                 │                      │
        ┌────────▼─────────┐   ┌────────▼──────────┐
        │   PostgreSQL     │   │  External APIs    │
        │   (Render)       │   │  DETER · INPE     │
        │                  │   │  Mapbox · SICAR   │
        └──────────────────┘   └───────────────────┘
```

## Getting Started

### Prerequisites

- Node.js 18+ and npm
- PostgreSQL 15+ (or a managed service like Render/Supabase)
- A Mapbox account (free tier available) for the access token

### Local Setup

**1. Clone the repository**

```bash
git clone https://github.com/Matheus-27mm/sela.git
cd sela
```

**2. Backend setup**

```bash
cd server
npm install
cp .env.example .env
# Edit .env with your DATABASE_URL, JWT_SECRET, etc.
npx prisma generate
npx prisma db push
npx prisma db seed   # creates demo users (optional)
npm run dev
```

The backend will be running at `http://localhost:3001`.

**3. Frontend setup**

```bash
cd ../client
npm install
cp .env.example .env
# Edit .env with VITE_API_URL and VITE_MAPBOX_TOKEN
npm run dev
```

The frontend will be running at `http://localhost:5178`.

### Environment Variables

**Backend (`server/.env`):**

```env
DATABASE_URL=postgresql://user:password@host:5432/sela
JWT_SECRET=<generate with: openssl rand -hex 64>
ALLOWED_ORIGINS=http://localhost:5178,https://your-frontend.vercel.app
PORT=3001
```

**Frontend (`client/.env`):**

```env
VITE_API_URL=http://localhost:3001
VITE_MAPBOX_TOKEN=pk.your_mapbox_public_token
```

> ⚠️ **Never commit `.env` files.** They are listed in `.gitignore` for a reason.

## Roadmap

### ✅ Phase 1 — MVP (Completed)
- [x] Producer, farm, and lot CRUD
- [x] Public traceability via QR code
- [x] DETER/INPE integration with real-time alerts
- [x] ESG Score engine
- [x] Cut-off date validation
- [x] Due Diligence PDF reports
- [x] Public landing page

### 🔵 Phase 2 — Pilot with Cooperatives (Q2 2026)
- [ ] Cooperative self-registration with admin approval
- [ ] Invitation system for auditors and producers
- [ ] Producer access via single-use links
- [ ] Real-time DETER validation on harvest registration
- [ ] CSV bulk import for producer onboarding
- [ ] Multi-language support (PT-BR, EN, ES)

### ⚪ Phase 3 — Scale (Q3-Q4 2026)
- [ ] Mobile app (PWA) for field technicians with GPS capture
- [ ] WhatsApp Business integration for producer notifications
- [ ] Automated weekly satellite scanning with email digests
- [ ] Public API for traceability verification by importers
- [ ] LGPD compliance documentation and DPA templates

## Project Structure

```
sela/
├── client/                      # React frontend (Vite)
│   ├── public/
│   │   └── landing/             # Public marketing landing page
│   ├── src/
│   │   ├── components/          # Sidebar, ExportButton, etc.
│   │   ├── pages/               # Dashboard, Farms, Lots, Map, etc.
│   │   ├── lib/                 # API client, helpers
│   │   └── App.jsx              # Router and layout
│   └── vite.config.js
├── server/                      # Node.js backend (Express + Prisma)
│   ├── prisma/
│   │   ├── schema.prisma        # Database schema
│   │   └── seed.js              # Demo data seeder
│   ├── services/                # Business logic
│   │   ├── auditService.js
│   │   ├── cutOffService.js
│   │   ├── deforestationService.js
│   │   ├── dueDiligenceReport.js
│   │   ├── esgScore.js
│   │   └── lotTraceabilityReport.js
│   └── index.js                 # Express app entry point
├── .gitignore
├── package.json
├── README.md                    # This file
└── README.pt-BR.md              # Portuguese version
```

## Contributing

This project is currently in private development. If you are part of a cooperative, regulatory body, or auditing firm interested in piloting SELA, please reach out via the contact below.

For technical contributions:
1. Open an issue describing the change
2. Wait for approval before opening a PR
3. Follow the existing code style (the codebase favors readability over patterns)

## License

Proprietary — All rights reserved. Contact the author for licensing inquiries.

## Author

**Matheus Margarido**
Computer Engineering student · Full-stack developer · AppSec aspirant
Manaus, Amazonas, Brazil

📧 [Add your professional email]
🔗 [LinkedIn](https://linkedin.com/in/matheusmargarido) · [GitHub](https://github.com/Matheus-27mm)

## Acknowledgments

- **INPE** for providing open access to the DETER deforestation alert system
- **MapBiomas** for land use historical data
- **OCB-AM** and **Cooperar** for ongoing dialogue on cooperative needs in the Amazon
- The open-source community behind React, Prisma, Mapbox, and PostgreSQL

---

<div align="center">

*Built in Manaus 🌿 with care for the Amazon and the people who make it thrive.*

</div>
