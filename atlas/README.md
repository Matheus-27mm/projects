# ATLAS OMEGA v3.2

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.11+-blue?style=flat-square&logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-latest-green?style=flat-square&logo=fastapi)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18+-61dafb?style=flat-square&logo=react)](https://react.dev)
[![Node.js](https://img.shields.io/badge/Node.js-20+-339933?style=flat-square&logo=nodedotjs)](https://nodejs.org)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

[🇧🇷 Português](#português) · [🇺🇸 English](#english)

</div>

---

## Português

### O que é

ATLAS é um **AI Agent pessoal** que roda localmente. Não é um wrapper de API — é uma arquitetura cognitiva completa com memória, aprendizado contínuo, monitoramento de segurança e interface holográfica inspirada no JARVIS do Iron Man.

### Stack

| Camada | Tecnologia |
|--------|-----------|
| Frontend | React 18 + Vite |
| Backend AI | FastAPI (Python 3.11) |
| Gateway | Node.js + WebSocket |
| Modelo de linguagem | Anthropic Claude |
| Voz | ElevenLabs |
| Mapas | Leaflet.js |

### Arquitetura

```
┌──────────────────────────────────────────────────────┐
│                  INTERFACE JARVIS                     │
│           React + HoloCanvas + Arc Reactor            │
└───────────────────────┬──────────────────────────────┘
                        │ WebSocket / SSE
┌───────────────────────▼──────────────────────────────┐
│               AI BRAIN (Claude)                       │
│          24 tools | Streaming | Contexto              │
└──────┬──────────┬───────────┬────────────────────────┘
       │          │           │
┌──────▼───┐ ┌───▼─────┐ ┌───▼──────────────────────┐
│  MEMÓRIA │ │  TWIN   │ │       GUARDIAN            │
│ Episódica│ │ Digital │ │  CVE Scanner (NVD API)    │
│ Learning │ │ Foco /  │ │  Evil Twin Detection      │
│ Knowledge│ │ Humor / │ │  Monitor de Rede          │
│ Graph    │ │ Energia │ │  ARP Scanner              │
└──────────┘ └─────────┘ └──────────────────────────┘
```

### Funcionalidades

#### 🧠 Inteligência Artificial
- **24 tools** — busca web, criação de arquivos, clima, Wolfram Alpha, geração de imagem, modelos 3D, OCR, análise de segurança, CVE lookup, OWASP
- **Streaming em tempo real** — respostas token por token via SSE
- **Memória episódica** — registra eventos com importância, tags e timestamp
- **Digital Twin** — modelo do estado cognitivo do usuário (foco, energia, humor, produtividade)
- **Expert System** — avalia situação e gera alertas inteligentes proativamente
- **Knowledge Graph** — grafo de entidades e relações semânticas
- **Inference Engine** — dedução lógica baseada em fatos acumulados
- **Rule Engine** — regras customizáveis que disparam ações automáticas
- **Planning Engine** — sugere próxima tarefa com base no estado cognitivo atual
- **Continuous Learning** — aprende fatos e preferências ao longo das sessões

#### 🛡️ Guardian (Segurança)
- Scan automático de CVEs a cada 15 minutos via NVD API oficial
- Detecção de vulnerabilidades críticas (CVSS ≥ 9.0)
- Alertas proativos no chat com contexto completo da vulnerabilidade
- Threat level 0–100 com visual dinâmico por cor
- Scan manual sob demanda

#### 📡 Ambient Scanner
- Detecção de todas as redes Wi-Fi ao redor com análise de segurança por rede
- **Evil Twin Detection** — identifica redes clonadas por fabricantes diferentes
- Scan ARP da rede local — mapeia todos os dispositivos conectados
- Mapa geolocalizado em tempo real com Leaflet + OpenStreetMap
- Histórico persistente de scans em JSON
- Alertas automáticos para redes abertas novas e dispositivos desconhecidos

#### 📊 Monitoramento do Sistema
- CPU, RAM, GPU e temperatura em tempo real via SSE
- Activity tracker — rastreia o contexto de trabalho ativo no PC
- Task Manager com prioridade, prazo e projeto
- Analytics dashboard com visão geral de todos os módulos
- Notificações agregadas de todas as fontes (Guardian, Expert System, Tasks)

#### 🔊 Voz
- TTS com ElevenLabs — 5 perfis contextuais (normal, alert, briefing, technical, casual)
- Speech Recognition em pt-BR
- Resposta automática por voz após cada mensagem

#### 📱 Multi-dispositivo
- Interface desktop completa
- Interface tablet otimizada para touch (acesso via `/?tablet`)
- Qualquer dispositivo na mesma rede pode acessar
- Sincronização em tempo real via WebSocket

### Estrutura do Projeto

```
atlas/
├── backend-python/              # FastAPI — AI Engine
│   └── src/
│       ├── main.py              # Rotas e lifecycle
│       ├── ai/                  # Claude Brain + System Prompt + 24 tools
│       ├── functions/           # Executores das tools
│       ├── voice/               # ElevenLabs TTS
│       ├── system/              # Monitor de sistema (psutil)
│       ├── core/                # Event Bus interno
│       ├── cognition/           # Twin, Memory, Expert, Graph, Rules, Inference, Planning
│       ├── guardian/            # CVE Scanner proativo
│       ├── scanner/             # Ambient Scanner Wi-Fi + ARP
│       └── data/                # Outputs, uploads, scanner_history.json
│
├── backend-node/                # Node.js — WebSocket Gateway
│   └── server.js                # WS /ws (chat) + WS /guardian (alertas)
│
└── frontend/                    # React + Vite
    └── src/
        ├── main.jsx             # Router (?tablet → TabletView)
        ├── App.jsx              # Interface principal
        └── components/
            ├── ScannerPanel.jsx # Scanner com 5 abas + mapa
            ├── TabletView.jsx   # Interface touch
            ├── Forge.jsx        # Visualizador 3D GLB
            ├── TaskPanel.jsx
            ├── AnalyticsPanel.jsx
            ├── ExpertPanel.jsx
            └── SplashScreen.jsx
```

### Instalação

```bash
# 1. Clone
git clone https://github.com/Matheus-27mm/atlas.git
cd atlas

# 2. Backend Python
cd backend-python
pip install -r requirements.txt
cp .env.example .env
# Edite .env com suas chaves

# 3. Gateway Node
cd ../backend-node
npm install

# 4. Frontend
cd ../frontend
npm install
```

**.env**
```env
ANTHROPIC_API_KEY=sua_chave_aqui
ELEVENLABS_API_KEY=sua_chave_aqui
ELEVENLABS_VOICE_ID=JBFqnCBsd6RMkjVDRZzb
```

### Iniciando

```bash
# Terminal 1 — rodar como Administrador para ARP scan funcionar
cd backend-python
python -m uvicorn src.main:app --host 0.0.0.0 --port 8005 --reload

# Terminal 2
cd backend-node
npm run dev

# Terminal 3
cd frontend
npm run dev
```

| Acesso | URL |
|--------|-----|
| Desktop | `http://localhost:5175` |
| Tablet | `http://SEU_IP:5175/?tablet` |
| API Docs | `http://localhost:8005/docs` |

### API

```
POST /api/ai/chat/stream         Chat com streaming (SSE)
GET  /api/system/stream          Métricas em tempo real (SSE)
GET  /api/cognition/twin         Estado do Digital Twin
GET  /api/cognition/episodes     Memória episódica
GET  /api/expert/evaluate        Avaliação do Expert System
POST /api/guardian/scan          Scan manual de CVEs
GET  /api/guardian/status        Status do Guardian
POST /api/scanner/ambient        Scan Wi-Fi + dispositivos
GET  /api/scanner/history        Histórico de scans
GET  /api/tasks                  Lista de tarefas
GET  /api/notifications          Notificações agregadas
POST /api/voice/speak            TTS
GET  /docs                       Swagger UI completo
```

### Roadmap

- [x] AI Brain com 24 tools
- [x] Arquitetura cognitiva completa (Twin, Memory, Expert, Graph, Rules, Inference, Planning)
- [x] Guardian proativo com CVE detection (NVD API)
- [x] Ambient Scanner com Evil Twin detection e histórico
- [x] Interface JARVIS holográfica (Arc Reactor animado)
- [x] Multi-dispositivo com interface tablet
- [ ] Memória vetorial (ChromaDB)
- [ ] Guardian + Scanner integrados com correlação de dados
- [ ] Integração com GitHub
- [ ] Resumo diário automático em voz
- [ ] ESP32 wearable — LED Arc Reactor físico no peito
- [ ] Wake word detection

---

## English

### What is it

ATLAS is a **personal AI Agent** that runs locally. It is not an API wrapper — it is a complete cognitive architecture with memory, continuous learning, security monitoring, and a holographic interface inspired by JARVIS from Iron Man.

### Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + Vite |
| AI Backend | FastAPI (Python 3.11) |
| Gateway | Node.js + WebSocket |
| Language Model | Anthropic Claude |
| Voice | ElevenLabs |
| Maps | Leaflet.js |

### Architecture

```
┌──────────────────────────────────────────────────────┐
│                  JARVIS INTERFACE                     │
│           React + HoloCanvas + Arc Reactor            │
└───────────────────────┬──────────────────────────────┘
                        │ WebSocket / SSE
┌───────────────────────▼──────────────────────────────┐
│               AI BRAIN (Claude)                       │
│          24 tools | Streaming | Context               │
└──────┬──────────┬───────────┬────────────────────────┘
       │          │           │
┌──────▼───┐ ┌───▼─────┐ ┌───▼──────────────────────┐
│  MEMORY  │ │  TWIN   │ │       GUARDIAN            │
│ Episodic │ │ Digital │ │  CVE Scanner (NVD API)    │
│ Learning │ │ Focus / │ │  Evil Twin Detection      │
│ Knowledge│ │ Mood /  │ │  Network Monitor          │
│ Graph    │ │ Energy  │ │  ARP Scanner              │
└──────────┘ └─────────┘ └──────────────────────────┘
```

### Features

#### 🧠 Artificial Intelligence
- **24 tools** — web search, file creation, weather, Wolfram Alpha, image generation, 3D models, OCR, security analysis, CVE lookup, OWASP
- **Real-time streaming** — token-by-token responses via SSE
- **Episodic memory** — logs events with importance, tags and timestamps
- **Digital Twin** — cognitive state model (focus, energy, mood, productivity streak)
- **Expert System** — evaluates context and generates proactive intelligent alerts
- **Knowledge Graph** — semantic entity and relationship graph
- **Inference Engine** — logical deduction from accumulated facts
- **Rule Engine** — customizable rules that trigger automatic actions
- **Planning Engine** — suggests next task based on current cognitive state
- **Continuous Learning** — learns facts and preferences across sessions

#### 🛡️ Guardian (Security)
- Automatic CVE scan every 15 minutes via official NVD API
- Critical vulnerability detection (CVSS ≥ 9.0)
- Proactive in-chat alerts with full vulnerability context
- Threat level 0–100 with dynamic color indicator
- On-demand manual scan

#### 📡 Ambient Scanner
- Detects all nearby Wi-Fi networks with per-network security analysis
- **Evil Twin Detection** — identifies cloned networks from different manufacturers
- Local network ARP scan — maps all connected devices
- Real-time geolocated map with Leaflet + OpenStreetMap
- Persistent scan history stored in JSON
- Automatic alerts for new open networks and unknown devices

#### 📊 System Monitoring
- CPU, RAM, GPU and temperature in real time via SSE
- Activity tracker — monitors active work context on the PC
- Task Manager with priority, deadline and project
- Analytics dashboard with overview of all modules
- Aggregated notifications from all sources (Guardian, Expert System, Tasks)

#### 🔊 Voice
- ElevenLabs TTS with 5 contextual profiles (normal, alert, briefing, technical, casual)
- Speech Recognition in pt-BR
- Automatic voice response after each message

#### 📱 Multi-device
- Full desktop interface
- Touch-optimized tablet interface (access via `/?tablet`)
- Any device on the same network can connect
- Real-time synchronization via WebSocket

### Project Structure

```
atlas/
├── backend-python/              # FastAPI — AI Engine
│   └── src/
│       ├── main.py              # Routes and lifecycle
│       ├── ai/                  # Claude Brain + System Prompt + 24 tools
│       ├── functions/           # Tool executors
│       ├── voice/               # ElevenLabs TTS
│       ├── system/              # System monitor (psutil)
│       ├── core/                # Internal Event Bus
│       ├── cognition/           # Twin, Memory, Expert, Graph, Rules, Inference, Planning
│       ├── guardian/            # Proactive CVE Scanner
│       ├── scanner/             # Ambient Scanner Wi-Fi + ARP
│       └── data/                # Outputs, uploads, scanner_history.json
│
├── backend-node/                # Node.js — WebSocket Gateway
│   └── server.js                # WS /ws (chat) + WS /guardian (alerts)
│
└── frontend/                    # React + Vite
    └── src/
        ├── main.jsx             # Router (?tablet → TabletView)
        ├── App.jsx              # Main interface
        └── components/
            ├── ScannerPanel.jsx # Scanner with 5 tabs + map
            ├── TabletView.jsx   # Touch interface
            ├── Forge.jsx        # 3D GLB viewer
            ├── TaskPanel.jsx
            ├── AnalyticsPanel.jsx
            ├── ExpertPanel.jsx
            └── SplashScreen.jsx
```

### Installation

```bash
# 1. Clone
git clone https://github.com/Matheus-27mm/atlas.git
cd atlas

# 2. Python backend
cd backend-python
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your API keys

# 3. Node gateway
cd ../backend-node
npm install

# 4. Frontend
cd ../frontend
npm install
```

**.env**
```env
ANTHROPIC_API_KEY=your_key_here
ELEVENLABS_API_KEY=your_key_here
ELEVENLABS_VOICE_ID=JBFqnCBsd6RMkjVDRZzb
```

### Running

```bash
# Terminal 1 — run as Administrator for ARP scan
cd backend-python
python -m uvicorn src.main:app --host 0.0.0.0 --port 8005 --reload

# Terminal 2
cd backend-node
npm run dev

# Terminal 3
cd frontend
npm run dev
```

| Access | URL |
|--------|-----|
| Desktop | `http://localhost:5175` |
| Tablet | `http://YOUR_IP:5175/?tablet` |
| API Docs | `http://localhost:8005/docs` |

### API Reference

```
POST /api/ai/chat/stream         Chat with streaming (SSE)
GET  /api/system/stream          Real-time system metrics (SSE)
GET  /api/cognition/twin         Digital Twin state
GET  /api/cognition/episodes     Episodic memory
GET  /api/expert/evaluate        Expert System evaluation
POST /api/guardian/scan          Manual CVE scan
GET  /api/guardian/status        Guardian status
POST /api/scanner/ambient        Wi-Fi + device scan
GET  /api/scanner/history        Scan history
GET  /api/tasks                  Task list
GET  /api/notifications          Aggregated notifications
POST /api/voice/speak            TTS
GET  /docs                       Full Swagger UI
```

### Roadmap

- [x] AI Brain with 24 tools
- [x] Full cognitive architecture (Twin, Memory, Expert, Graph, Rules, Inference, Planning)
- [x] Proactive Guardian with CVE detection (NVD API)
- [x] Ambient Scanner with Evil Twin detection and persistent history
- [x] Holographic JARVIS interface (animated Arc Reactor)
- [x] Multi-device with tablet interface
- [ ] Vector memory (ChromaDB)
- [ ] Guardian + Scanner integration with data correlation
- [ ] GitHub integration
- [ ] Automatic daily voice summary
- [ ] ESP32 wearable — physical Arc Reactor LED on chest
- [ ] Wake word detection

---

## License

MIT
