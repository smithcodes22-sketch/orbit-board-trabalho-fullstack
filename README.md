# OrbitBoard — Integração Full Stack

Aplicação full stack para gestão de projetos, tarefas e equipe. Desenvolvida como trabalho final do **Módulo 5 — Integração Full Stack** (Capacitação em IA e Transformação Digital).

## Integrantes da equipe

| Integrante | Contribuição |
|---|---|
| _Nome 1_ | _preencher_ |
| _Nome 2_ | _preencher_ |
| _Nome 3_ | _preencher_ |

## Descrição

O OrbitBoard permite acompanhar projetos, gerenciar tarefas com filtros e status, visualizar métricas no dashboard e consultar membros da equipe. Os dados são mantidos **em memória** para concentrar o aprendizado na integração HTTP/JSON entre front-end e API.

## Arquitetura

```text
Navegador → Front-end (React/Nginx) → API (ASP.NET Core) → Dados em memória
```

Detalhes em [docs/arquitetura.md](./docs/arquitetura.md).

## Tecnologias

| Camada | Stack |
|---|---|
| Front-end | React 18, Vite, React Router, Nginx (produção) |
| Back-end | ASP.NET Core 8, Swagger, ProblemDetails |
| Infra | Docker, Docker Compose |
| Versionamento | Git, GitHub |

## Pré-requisitos

**Execução local:**

- [.NET SDK 8](https://dotnet.microsoft.com/download)
- [Node.js 20+](https://nodejs.org/)

**Execução com Docker:**

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) ou Docker Engine + Compose

## Como executar

### Opção 1 — Docker Compose (recomendado)

Na raiz do projeto:

```bash
docker compose up --build
```

Aguarde os containers ficarem saudáveis. Acesse:

| Serviço | URL |
|---|---|
| Front-end | http://localhost:8080 |
| API | http://localhost:5200 |
| Swagger | http://localhost:5200/swagger |
| Health check (API) | http://localhost:5200/health |
| Health check (front) | http://localhost:8080/health |

Parar os containers:

```bash
docker compose down
```

### Opção 2 — Execução local

**Back-end** (terminal 1):

```bash
cd backend
dotnet restore OrbitBoard.Api.sln
dotnet run --project OrbitBoard.Api
```

**Front-end** (terminal 2):

```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

| Serviço | URL |
|---|---|
| Front-end | http://localhost:5173 |
| API | http://localhost:5200 |
| Swagger | http://localhost:5200/swagger |

## Endpoints principais

| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/health` | Health check da API |
| GET | `/api/dashboard` | Métricas e tarefas recentes |
| GET/POST/PUT/DELETE | `/api/projects` | CRUD de projetos |
| GET/POST/PUT/PATCH/DELETE | `/api/tasks` | CRUD e filtros de tarefas |
| GET | `/api/team-members` | Lista da equipe |

Contrato completo: [docs/contrato-api.md](./docs/contrato-api.md)

## Variáveis de ambiente

Consulte [.env.example](./.env.example) na raiz e [frontend/.env.example](./frontend/.env.example).

| Variável | Onde | Descrição |
|---|---|---|
| `VITE_API_URL` | Front-end | URL da API (`http://localhost:5200`) |
| `Cors__AllowedOrigins` | Back-end | Origens CORS separadas por vírgula |
| `ASPNETCORE_URLS` | Back-end (Docker) | URL interna do container |

## Documentação

| Documento | Conteúdo |
|---|---|
| [docs/arquitetura.md](./docs/arquitetura.md) | Diagrama e decisões técnicas |
| [docs/contrato-api.md](./docs/contrato-api.md) | Endpoints, payloads e códigos HTTP |
| [docs/evidencias-testes.md](./docs/evidencias-testes.md) | Roteiro de testes e evidências |
| [docs/roteiro-apresentacao.md](./docs/roteiro-apresentacao.md) | Roteiro da apresentação final |
| [backend/README.md](./backend/README.md) | Detalhes do back-end |
| [frontend/README.md](./frontend/README.md) | Detalhes do front-end |

## Estrutura do repositório

```text
orbit-board-project/
├── backend/           API ASP.NET Core + Dockerfile
├── frontend/          React + Vite + Dockerfile
├── docs/              Documentação do trabalho
├── docker-compose.yml Orquestração dos containers
├── .env.example       Variáveis de ambiente
├── .gitignore
└── README.md
```

## Ajustes realizados pela equipe

- Dockerfiles multi-stage para back-end e front-end
- `docker-compose.yml` com health check e dependência entre serviços
- CORS configurável via variável de ambiente (suporte a dev e Docker)
- Documentação completa em `docs/` e README de entrega
- Pasta de evidências de testes com roteiro manual

## Fluxo Git (equipe)

Este repositório deve ser a cópia **própria da equipe** no GitHub. Para sincronizar com o repositório base do docente:

```bash
git remote add upstream https://github.com/denkencapacitacao/orbit-board-project.git
git fetch upstream
git merge upstream/main
```

Trabalhe em branches (`feature/nome-da-atividade`) e integre via Pull Request.
