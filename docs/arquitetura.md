# Arquitetura — OrbitBoard

## Visão geral

O **OrbitBoard** é uma aplicação full stack para acompanhamento de projetos, tarefas e equipe. Foi utilizada como estudo de caso do Módulo 5 (Integração Full Stack), com foco em integração HTTP/JSON, containers e documentação.

## Diagrama

```text
┌─────────────────────────────────────────────────────────────┐
│                        Navegador                            │
│                   http://localhost:8080                     │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTP (JSON)
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Front-end (React + Vite → Nginx)                           │
│  - Dashboard, Projetos, Tarefas, Equipe                     │
│  - Cliente HTTP em src/api/client.js                        │
│  Porta host: 8080 | Porta container: 80                     │
└──────────────────────────┬──────────────────────────────────┘
                           │ fetch → VITE_API_URL
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Back-end (ASP.NET Core 8)                                  │
│  - Controllers REST + Swagger                               │
│  - Middleware de erros (ProblemDetails)                     │
│  - CORS configurável por variável de ambiente               │
│  Porta host: 5200 | Porta container: 8080                   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Dados em memória (WorkspaceService)                        │
│  - Projetos, tarefas e membros pré-carregados               │
│  - Recriados a cada reinício da API                         │
└─────────────────────────────────────────────────────────────┘
```

## Camadas

### Front-end

- **Tecnologia:** React 18, Vite, React Router.
- **Responsabilidade:** Interface do usuário, consumo da API, tratamento de estados (carregamento, sucesso, erro).
- **Comunicação:** Requisições `fetch` com corpo JSON para `VITE_API_URL`.

### Back-end

- **Tecnologia:** ASP.NET Core 8, Swashbuckle (Swagger).
- **Responsabilidade:** Expor endpoints REST, validar entradas, aplicar regras de negócio e retornar JSON.
- **Organização:** Controllers → Services → Models (dados em memória).

### Infraestrutura

- **Docker Compose** orquestra dois serviços: `backend` e `frontend`.
- O front-end em produção é servido por **Nginx** (SPA com fallback para `index.html`).
- Health checks: `/health` na API e `/health` no Nginx do front-end.

## Fluxo de uma requisição (exemplo: criar tarefa)

1. Usuário preenche o formulário na tela de Tarefas.
2. O front-end chama `POST /api/tasks` via `api.tasks.create()`.
3. A API valida o payload, persiste em memória e retorna `201` com o objeto criado.
4. O front-end atualiza a listagem ou exibe mensagem de erro (`400`, `409`, etc.).

## Decisões técnicas

| Decisão | Motivo |
|---|---|
| Dados em memória | Simplifica o escopo didático; foco na integração HTTP |
| CORS via configuração | Permite desenvolvimento local (`5173`) e Docker (`8080`) |
| ProblemDetails nos erros | Padrão consistente entre API e front-end |
| Build multi-stage | Imagens menores e builds reproduzíveis |
