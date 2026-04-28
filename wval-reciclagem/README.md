# WVAL Reciclagem

Sistema de gestão completo para cooperativas e empresas de reciclagem. Controle de entradas, pesagens, pagamentos, comissões e relatórios — com leitura automática de notas de balança via inteligência artificial.

🌐 **Sistema em produção:** [wvalreciclagem.com.br](https://wvalreciclagem.com.br)

---

## ✨ Funcionalidades

### 📥 Operações
- **Nova Entrada** com cálculo automático de peso bruto, líquido e comissão
- **Scanner de Notas de Balança via IA** — tire uma foto e a Claude AI lê tudo (fornecedor, produtos, pesagens, bags)
- **Múltiplas pesagens por produto** com marcação individual de bags
- **Cálculo automático de desconto de bags** (2kg por bag) com distribuição proporcional
- **Edição completa de movimentações** (data, fornecedor, itens, preços)
- **Recibo em PDF** gerado automaticamente após salvar

### 💰 Financeiro
- **Pagamentos e Adiantamentos** com QR Code PIX automático
- **Filtros por período** (hoje, semana, mês, últimos 30 dias)
- **Editar pagamentos** com persistência completa
- **Histórico filtrado por fornecedor** com detalhamento de cada entrega
- **Anexar comprovantes** (imagem ou PDF)
- **Tabela de pendentes** com saldo devedor real

### 👥 Cadastros
- Fornecedores com chave PIX, telefone, endereço
- Produtos com preços padrão por kg
- Preços diferenciados por fornecedor
- Compradores de papelão

### 📊 Visualização
- Dashboard com métricas em tempo real
- Top fornecedores e produtos
- Distribuição de produtos
- Avisos e alertas operacionais

### 🔐 Segurança
- Autenticação JWT com expiração de 30 dias
- Auto-redirect para login em caso de sessão expirada
- Helmet + Rate Limiting + CORS configurado
- Tokens aceitos via header ou query string (para downloads)

### 📱 Mobile / PWA
- Progressive Web App instalável
- Bottom Navigation nativa estilo app
- Layout responsivo otimizado
- Funciona offline (cache de assets)

---

## 🏗️ Stack

### Frontend
- HTML5 + JavaScript vanilla (zero dependências de build)
- Inter, Outfit e Space Mono via Google Fonts
- QRCode.js para geração de PIX
- SheetJS (XLSX) para exportação de planilhas
- Service Worker para PWA

### Backend
- Node.js + Express
- PostgreSQL via Supabase
- JWT para autenticação
- bcrypt para hash de senhas
- PDFKit para geração de recibos
- ExcelJS para exportação Excel
- Anthropic API (Claude Sonnet 4) para OCR de notas

### Infraestrutura
- Render (plano pago, sem hibernação)
- Supabase (PostgreSQL gerenciado)
- Domínio: wvalreciclagem.com.br

---

## 📁 Estrutura

```
wval-reciclagem/
├── backend/
│   ├── src/
│   │   ├── controllers/      # Lógica de cada rota
│   │   ├── services/         # Regras de negócio + acesso ao banco
│   │   ├── middlewares/      # Auth, permissões
│   │   ├── routes/           # Definição de rotas
│   │   └── db/               # Inicialização e conexão PostgreSQL
│   ├── server.js             # Entrada do Express
│   └── package.json
└── frontend/
    ├── login.html            # Tela de login com vídeo de fundo
    ├── dashboard.html        # Painel principal
    ├── movimentacao.html     # Nova entrada + scanner OCR
    ├── movimentacoes.html    # Histórico com filtros
    ├── pagamentos.html       # Gestão financeira
    ├── fornecedores.html
    ├── produtos.html
    ├── papelao.html
    ├── comissoes.html
    ├── metas.html
    ├── relatorios.html
    ├── configuracoes.html
    ├── js/
    │   └── auth-check.js     # Interceptador global de 401
    ├── videos/
    │   └── bg.mp4            # Vídeo de fundo do login
    ├── icons/                # Ícones do PWA
    ├── manifest.json
    └── service-worker.js
```

---

## 🚀 Como rodar localmente

### Pré-requisitos
- Node.js 18+
- PostgreSQL (ou conta no Supabase)
- Conta na Anthropic com API key (para o scanner)

### Instalação

```bash
# Clonar repositório
git clone https://github.com/Matheus-27mm/wval-reciclagem.git
cd wval-reciclagem

# Instalar dependências do backend
cd backend
npm install
```

### Configuração

Crie um arquivo `.env` em `backend/` com:

```env
PORT=3001
NODE_ENV=development
DATABASE_URL=postgresql://user:senha@host:5432/database
JWT_SECRET=sua-chave-secreta-aqui
ANTHROPIC_API_KEY=sk-ant-api03-...
```

### Iniciar

```bash
# Backend (porta 3001 — serve API + frontend estático)
cd backend
node server.js
```

Acesse `http://localhost:3001`

---

## 🔐 Variáveis de ambiente

| Variável | Descrição | Obrigatório |
|----------|-----------|-------------|
| `PORT` | Porta do servidor | Não (default 3001) |
| `NODE_ENV` | `development` ou `production` | Não |
| `DATABASE_URL` | Connection string PostgreSQL | Sim |
| `JWT_SECRET` | Chave secreta para JWT | Sim |
| `ANTHROPIC_API_KEY` | Chave da Anthropic API | Sim (para scanner) |

---

## 📌 Rotas principais da API

### Autenticação
- `POST /api/auth/login`
- `POST /api/auth/register`

### Movimentações
- `GET /api/movimentacoes`
- `POST /api/movimentacoes`
- `GET /api/movimentacoes/:id`
- `PUT /api/movimentacoes/:id`
- `DELETE /api/movimentacoes/:id`
- `GET /api/movimentacoes/:id/receipt` (PDF)
- `GET /api/movimentacoes/:id/receipt/whatsapp`

### Pagamentos
- `GET /api/payments`
- `POST /api/payments`
- `DELETE /api/payments/:id`
- `GET /api/payments/balance/:supplier_id`

### Fornecedores
- `GET /api/suppliers`
- `POST /api/suppliers`
- `PUT /api/suppliers/:id`
- `DELETE /api/suppliers/:id`

### Scanner OCR
- `POST /api/scanner/ler-nota`

### Dashboard
- `GET /api/dashboard`
- `GET /api/dashboard/metrics`
- `GET /api/dashboard/alerts`

---

## 🧠 Scanner OCR de Notas

O scanner aceita fotos de notas de balança manuscritas e impressas. Funciona melhor com:

- Boa iluminação
- Letra de imprensa (cursiva pode causar erros pontuais)
- Foto enquadrada (sem cortes nas bordas)

### Como funciona
1. Usuário tira foto ou seleciona da galeria
2. Frontend envia base64 para `/api/scanner/ler-nota`
3. Backend chama Claude Sonnet 4 via Anthropic API
4. IA retorna JSON estruturado: fornecedor, data, produtos com pesagens, qtd de bags
5. Modal de confirmação permite revisar e corrigir antes de aplicar
6. Sistema preenche o formulário automaticamente com bags distribuídas proporcionalmente

---

## 📦 Deploy

O projeto está configurado para deploy direto no Render:

```bash
# Build command (se necessário)
cd backend && npm install

# Start command
cd backend && node server.js
```

O frontend é servido estaticamente pelo próprio Express na pasta `frontend/`.

---

## ⚠️ Observações importantes

- **PWA no iOS:** Após instalar o ícone na tela inicial, o usuário precisa fazer login *dentro* do PWA — o Safari e o PWA têm `localStorage` isolados.
- **Domínio correto:** `wvalreciclagem.com.br` (com `.br`, não esquecer)
- **Token JWT:** 30 dias de duração, com auto-redirect global em caso de 401.
- **Fuso horário:** Datas exibidas em `America/Manaus` (UTC-4).

---

## 📝 Licença

Projeto privado desenvolvido para WVAL Reciclagem. Todos os direitos reservados.

---

## 👨‍💻 Autor

**Matheus Menezes Margarido**
- GitHub: [@Matheus-27mm](https://github.com/Matheus-27mm)

---
