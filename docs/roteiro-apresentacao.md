# Roteiro de apresentação — OrbitBoard

## 1. Abertura

- Apresentar o OrbitBoard como aplicação full stack para gestão de projetos, tarefas e equipe.
- Explicar que o objetivo do trabalho foi demonstrar integração HTTP/JSON entre front-end React e API ASP.NET Core.

## 2. Arquitetura

- Mostrar o fluxo: navegador, front-end React/Nginx, API ASP.NET Core e dados em memória.
- Apontar os arquivos principais: `docker-compose.yml`, `backend/Dockerfile`, `frontend/Dockerfile` e `.env.example`.

## 3. Backend/API

- Abrir `http://localhost:5200/swagger`.
- Mostrar o health check em `http://localhost:5200/health`.
- Demonstrar os grupos de endpoints: dashboard, projetos, tarefas e membros da equipe.
- Explicar o tratamento de erros com `ProblemDetails`.

## 4. Frontend

- Abrir `http://localhost:8080` no Docker ou `http://localhost:5173` em execução local.
- Mostrar o dashboard com métricas consumidas da API.
- Mostrar listagem de projetos e tarefas.
- Demonstrar filtros, criação, edição, alteração de status e exclusão.

## 5. Docker

- Executar ou mostrar o comando:

```bash
docker compose up --build
```

- Mostrar os containers da API e do front-end rodando.
- Mostrar logs com:

```bash
docker compose logs
```

## 6. Testes e evidências

- Mostrar `docs/evidencias-testes.md`.
- Explicar as validações realizadas: build do backend, build do frontend, compose config e docker compose build.
- Apontar prints ou registros adicionados pela equipe, caso existam.

## 7. CI/CD

- Mostrar `.github/workflows/ci.yml`.
- Explicar que o pipeline valida backend, frontend e Docker Compose em push, pull request ou execução manual.

## 8. Encerramento

- Reforçar que o projeto entrega integração full stack com documentação, Docker, Swagger, CORS, README, evidências e CI/CD.
