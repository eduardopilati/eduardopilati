# Eduardo Pilati

### Engenheiro de Software · Especialista C#/.NET · 11+ anos

Transformo regras de negócio complexas — fiscal, pagamentos, logística, IA — em
**plataformas multi-tenant que escalam**, e opero a infraestrutura que as sustenta.
Construo de ponta a ponta: da arquitetura em Vertical Slice ao React, passando por
mensageria durável, isolamento por Row-Level Security e observabilidade própria.
Trato entrega de software como problema de sistema distribuído — idempotência,
outbox transacional e testes como gate de qualidade.

📍 Passo Fundo, RS · Brasil · 🌐 **[pilati.dev](https://pilati.dev)**

![.NET](https://img.shields.io/badge/.NET-512BD4?style=flat&logo=dotnet&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat&logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-425CC7?style=flat&logo=opentelemetry&logoColor=white)

---

## 🧩 Como eu construo software

Há um padrão que se repete nos meus projetos — uma assinatura de arquitetura:

- **Vertical Slice Architecture + CQRS** com Mediator gerado por *source generator*; o
  isolamento entre features é garantido por *architecture tests*, não só por convenção.
- **Multi-tenancy com Row-Level Security no PostgreSQL** como fronteira primária de
  isolamento (FORCE RLS + GUCs de sessão), testado contra vazamento entre tenants.
- **Mensageria durável** com Wolverine + RabbitMQ e **outbox/inbox transacional no
  Postgres** — idempotência, retries e dead-letter de verdade, com semântica exactly-once.
- **Hosts separados** (API · Admin · Worker) sobre uma mesma camada de aplicação e uma
  mesma espinha de observabilidade.
- **Contratos OpenAPI-first**, com clientes TypeScript gerados e paridade garantida por
  *snapshot tests*.
- **Teste como gate** — bancos PostgreSQL efêmeros reais e centenas de testes
  determinísticos por projeto.
- **Observabilidade que eu mesmo opero** — OpenTelemetry + Serilog → Prometheus, Grafana,
  Loki e Tempo. Sem depender de SaaS.
- **Backups automatizados** com restic, criptografados e versionados.

---

## 🚀 Projetos em destaque

### 🤖 [LumionAI](https://github.com/LumionAI) — atendimento no WhatsApp com IA auditável
> SaaS multi-tenant · .NET 10 · React 19 · *em desenvolvimento ativo*

Atendimento no WhatsApp em que **a IA só responde quando tem confiança suficiente** —
abaixo do limite configurado por cliente, a conversa é encaminhada para um humano com todo
o contexto. Cada decisão da IA (intenção, score de recuperação, confiança, modelo, tokens)
vira um registro **imutável**, pensado para auditoria e LGPD.

- Pipeline com *confidence gating*: classificação de intenção → RAG → geração → porta de confiança → responde ou escala
- Fila e atribuição de atendimento humano com RBAC e inbox em tempo real (Server-Sent Events)
- Isolamento por tenant via **PostgreSQL Row-Level Security**, com testes que provam o bloqueio cross-tenant
- Ingestão assíncrona resiliente: webhook Gupshup (HMAC tempo-constante) → Wolverine → RabbitMQ → worker idempotente

**Stack:** C#/.NET 10 · ASP.NET Core · EF Core 10 · Wolverine + RabbitMQ · PostgreSQL (RLS) · OpenAI + RAG · React 19 + Vite + TypeScript · OpenTelemetry/Grafana

### 💎 Ourivus — e-commerce de joias sob medida com processamento 3D (STL)
> E-commerce + manufatura · .NET · React (SSR) · *em produção*

Venda de peças personalizadas a partir de modelos 3D: o cliente envia um arquivo `.stl`,
customiza a peça e recebe **preço e prazo calculados em tempo real**. Nos bastidores, a
plataforma cobre todo o ciclo — pagamento, fiscal e logística — em um fluxo automatizado.

- **Processamento 3D in-process:** o worker calcula volume, peso e preço direto do STL (renderização *headless* via OSMesa, código nativo) — sem serviço externo
- **Fiscal:** emissão e sincronização de NF-e via Bling/SEFAZ
- **Pagamentos:** Mercado Pago com webhooks idempotentes
- **Logística:** cotação, etiquetas e rastreamento dos Correios
- **Mensageria durável:** outbox/inbox transacional (Wolverine + PostgreSQL) com retries e dead-letter
- Armazenamento de objetos nativo na **Oracle Cloud (OCI)**

**Stack:** C#/.NET · Vertical Slice + CQRS · EF Core + PostgreSQL · RabbitMQ · React (SSR) + TypeScript · OCI

### 🎫 Credenciamento Machine — credenciamento e presença para eventos
> Sistema operacional sob medida · .NET · Next.js · PostgreSQL · *em produção*

Substitui o controle manual em planilhas por um fluxo **integrado e auditável**. Credencia
colaboradores em cargos, dias, turnos e equipes, registra presença e gera relatórios de
credenciamento, presença e pagamento.

- Matriz de **cargos × dias × turnos × equipes**, com limites por turno
- Credenciamento individual ou em lote via planilha Excel, com **processamento parcial** (linhas válidas entram mesmo se outras falham) e relatório de erro por registro
- Check-in rápido em modo *kiosk* (QR/código) ou por link com token assinado
- Contratos com **assinatura eletrônica**; relatórios exportáveis (CSV/Excel)
- Jobs agendados (Quartz), upload via *presigned URL* no OCI, RBAC granular por ação

**Stack:** C#/.NET (ASP.NET Core) · EF Core + PostgreSQL · Quartz · OCI · Next.js (App Router) + TypeScript · OpenTelemetry + Grafana

### 🏭 Loja BQZ — e-commerce industrial B2B/B2C com backoffice forte
> E-commerce + operação fiscal/logística · .NET · React (SSR) · *em produção*

Unifica comercialização, operação e fiscal de uma empresa industrial em um único sistema,
atendendo vendas **B2B e B2C**. Combina catálogo técnico, ciclo de orçamento e expedição
operacional integrada — construída sobre a mesma base do Ourivus.

- **Catálogo industrial:** SKUs com variações, especificações técnicas, NCM e integração com ERP
- **Vendas B2B/B2C:** carrinho, orçamentos com pagamento de sinal e aprovação de vendedor, cupons
- **Fiscal:** emissão e reconciliação de NF-e via Bling, com **ICMS/DIFAL calculado por estado**
- **Expedição e picking:** fila de separação, pacotes, etiquetas ZPL (impressora térmica), declaração de conteúdo e rastreamento
- **Estoque:** disponibilidade, reposição e transferência entre estoques
- **CMS de layout:** versões de layout, menus, banners, carrosséis, grids e páginas customizadas

**Stack:** C#/.NET · Vertical Slice + CQRS · EF Core + PostgreSQL · RabbitMQ (outbox/inbox) · React (SSR) + TypeScript · Bling · Mercado Pago · Correios · OCI

### Outros projetos

- **DevMaestro** — Engine *self-hosted* de orquestração de workflows de IA para desenvolvimento.
  Runtime durável com **zero estado em memória**: cada nó, execução e espera externa é uma
  entidade no PostgreSQL, tornando workflows reexecutáveis e à prova de *restart*. Recuperação de
  eventos externos perdidos (GitHub Actions) via webhook + polling, com outbox transacional para
  semântica *exactly-once*. — *.NET · PostgreSQL + pgvector · SignalR · Claude Code / Codex CLI*
- **BitAI** — Inteligência operacional para redes de restaurantes sobre o **Toast POS**.
  Sincroniza dados via OAuth 2.0 (histórico de 5 anos) e expõe relatórios consultáveis em
  **linguagem natural** por um framework próprio de *tools* MCP sobre a OpenAI.
  — *.NET · MassTransit + RabbitMQ · OpenAI/MCP · Angular*
- **Crisiz** — SaaS comercial completo: **billing Stripe** com webhooks idempotentes e
  dead-letter, programa de afiliados com apuração de comissão, push via Firebase e três hosts
  (API/Worker/Admin) sobre a mesma espinha de observabilidade.
  — *.NET · Stripe · Firebase · React + Cloudflare Workers*

---

## 🧰 Stack

| Camada | Tecnologias |
| --- | --- |
| **Backend** | C# · .NET 10 · ASP.NET Core · Vertical Slice + CQRS (Mediator) · EF Core · FluentValidation · Identity |
| **Frontend** | React · Next.js · TypeScript · Vite · Tailwind · shadcn/ui · TanStack Query |
| **Dados & mensageria** | PostgreSQL (RLS · JSONB · pgvector) · Redis · RabbitMQ · Wolverine · outbox/inbox · Quartz |
| **Cloud, DevOps & observabilidade** | Docker · GHCR · Oracle Cloud (OCI) · Cloudflare · OpenTelemetry · Grafana · Loki · Tempo · Prometheus · Serilog |
| **Integrações** | Stripe · Mercado Pago · Bling/NF-e · Correios · WhatsApp (Meta/Gupshup) · Firebase |
| **IA aplicada** | OpenAI · RAG · Model Context Protocol (MCP) · *confidence gating* + auditoria |

---

## 📊 Atividade de código

<p align="center">
  <a href="https://codetime.dev"><img src="https://codetime.dev/api/widgets/donut.svg?uid=36619&days=30&limit=6&theme=light" alt="CodeTime"></a>
</p>

---

## 🔓 Open source & hobby

A maior parte do meu trabalho é privada (produtos e código de clientes), mas há código público:

- [**PilatiPlugins**](https://github.com/orgs/PilatiPlugins/repositories) — plugins de Minecraft (Spigot/Java)
- [**problems**](https://github.com/eduardopilati/problems) — soluções de programação competitiva (C/C++)
- [**omarchy-config**](https://github.com/eduardopilati/omarchy-config) — meu setup de Hyprland

---

## 📫 Contato

[![pilati.dev](https://img.shields.io/badge/pilati.dev-512BD4?style=for-the-badge&logo=googlechrome&logoColor=white)](https://pilati.dev)
