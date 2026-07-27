# Roteiro de apresentação — OrbitBoard

Duração sugerida: **8 a 12 minutos**

---

## 1. Abertura (1 min)

- Nome do projeto: **OrbitBoard**
- Integrantes da equipe e papéis
- Objetivo didático: demonstrar integração full stack

---

## 2. Finalidade da aplicação (1 min)

- Gestão de projetos, tarefas e equipe
- Dados em memória para foco na integração HTTP/JSON
- Fluxos principais: dashboard, CRUD de projetos/tarefas, filtros

---

## 3. Arquitetura (2 min)

Mostrar diagrama em [arquitetura.md](./arquitetura.md):

- **Front-end:** React + Vite (dev) / Nginx (Docker)
- **Back-end:** ASP.NET Core 8 + Swagger
- **Dados:** WorkspaceService em memória
- **Infra:** Docker Compose com dois serviços

Destacar: navegador → front → API → serviço em memória

---

## 4. Demonstração da aplicação (2 min)

1. Abrir front-end (`http://localhost:8080` ou `5173`)
2. Mostrar dashboard
3. Criar um projeto
4. Criar uma tarefa vinculada
5. Alterar status de uma tarefa
6. (Opcional) Provocar erro 409 ou mostrar filtro

---

## 5. Demonstração da API (1,5 min)

1. Abrir Swagger (`http://localhost:5200/swagger`)
2. Executar `GET /api/dashboard` ou `GET /api/projects`
3. Mostrar resposta JSON
4. Mencionar health check em `/health`

---

## 6. Docker Compose (1,5 min)

Explicar:

- `docker compose up --build`
- Portas: API `5200`, front `8080`
- Variáveis: `VITE_API_URL`, `Cors__AllowedOrigins`
- Health check do backend antes do front subir

Mostrar `docker compose ps` ou logs se possível.

---

## 7. Testes e evidências (1 min)

- Referenciar [evidencias-testes.md](./evidencias-testes.md)
- Mostrar 2–3 prints (Network tab, Swagger, dashboard)
- Mencionar cenários de erro testados

---

## 8. Ajustes realizados pela equipe (1 min)

Exemplos do que a equipe pode citar:

- Dockerfiles multi-stage (backend e frontend)
- `docker-compose.yml` com health check
- CORS configurável para dev e Docker
- Documentação em `docs/` e README completo

---

## 9. Dificuldades e soluções (1 min)

| Dificuldade | Como resolvemos |
|---|---|
| CORS no Docker | Origens configuráveis por variável de ambiente |
| URL da API no build do Vite | `VITE_API_URL` como build arg no Dockerfile |
| _Outras da equipe_ | |

---

## 10. Contribuição dos integrantes (30 s)

| Integrante | Contribuição |
|---|---|
| _Nome 1_ | _Ex.: Docker, backend_ |
| _Nome 2_ | _Ex.: Front-end, testes_ |
| _Nome 3_ | _Ex.: Documentação, apresentação_ |

---

## Checklist antes de apresentar

- [ ] Aplicação sobe sem erros (local ou Docker)
- [ ] Swagger acessível
- [ ] Front consome API ao vivo
- [ ] Prints/evidências prontos
- [ ] Divisão de fala combinada entre integrantes
