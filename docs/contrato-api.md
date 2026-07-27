# Contrato da API — OrbitBoard

Base URL local: `http://localhost:5200`  
Swagger: `http://localhost:5200/swagger`  
Health check: `http://localhost:5200/health`

Todas as respostas de sucesso usam `application/json`. Erros retornam `application/problem+json` (RFC 7807).

## Códigos de status comuns

| Código | Situação |
|---|---|
| `200` | Consulta ou atualização bem-sucedida |
| `201` | Recurso criado |
| `204` | Exclusão bem-sucedida (sem corpo) |
| `400` | Validação ou dados inválidos |
| `404` | Recurso não encontrado |
| `409` | Conflito de regra (ex.: nome duplicado, projeto com tarefas) |
| `500` | Erro interno |

---

## Dashboard

### `GET /api/dashboard`

Retorna métricas gerais e tarefas recentes.

**Resposta 200:**

```json
{
  "totalProjects": 3,
  "activeProjects": 2,
  "totalTasks": 12,
  "completedTasks": 4,
  "overdueTasks": 1,
  "recentTasks": [ /* WorkItemResponse[] */ ],
  "tasksByStatus": { "Backlog": 3, "InProgress": 5 }
}
```

---

## Projetos

### `GET /api/projects`

Lista todos os projetos.

### `GET /api/projects/{id}`

Consulta um projeto pelo GUID.

### `POST /api/projects`

Cria um projeto.

**Corpo:**

```json
{
  "name": "Portal do Cliente",
  "description": "Redesign do portal",
  "status": "Active",
  "startDate": "2026-07-01",
  "dueDate": "2026-12-31",
  "ownerId": "GUID_DO_MEMBRO"
}
```

### `PUT /api/projects/{id}`

Atualiza um projeto existente (mesmo schema do POST).

### `DELETE /api/projects/{id}`

Exclui um projeto **somente se não houver tarefas vinculadas** (`409` caso contrário).

---

## Tarefas

### `GET /api/tasks`

Lista tarefas com filtros opcionais:

| Parâmetro | Tipo | Descrição |
|---|---|---|
| `projectId` | GUID | Filtra por projeto |
| `status` | enum | Backlog, InProgress, InReview, Done |
| `priority` | enum | Low, Medium, High, Critical |
| `assigneeId` | GUID | Filtra por responsável |
| `search` | string | Busca no título/descrição |

### `GET /api/tasks/{id}`

Consulta uma tarefa pelo GUID.

### `POST /api/tasks`

Cria uma tarefa.

**Corpo:**

```json
{
  "projectId": "GUID_DO_PROJETO",
  "title": "Revisar integração",
  "description": "Validar fluxo front/back",
  "status": "Backlog",
  "priority": "High",
  "assigneeId": "GUID_DO_MEMBRO",
  "dueDate": "2026-08-20",
  "estimatedHours": 8
}
```

### `PUT /api/tasks/{id}`

Atualiza uma tarefa (mesmo schema do POST).

### `PATCH /api/tasks/{id}/status`

Altera apenas o status.

**Corpo:**

```json
{ "status": "InProgress" }
```

### `DELETE /api/tasks/{id}`

Exclui uma tarefa.

---

## Equipe

### `GET /api/team-members`

Lista os integrantes da equipe com id, nome, e-mail e função.

---

## Health

### `GET /health`

**Resposta 200:**

```json
{
  "status": "healthy",
  "service": "OrbitBoard.Api",
  "utcTime": "2026-07-27T14:00:00+00:00"
}
```

---

## Enums

**ProjectStatus:** `Planning`, `Active`, `OnHold`, `Completed`, `Cancelled`

**WorkItemStatus:** `Backlog`, `InProgress`, `InReview`, `Done`

**WorkItemPriority:** `Low`, `Medium`, `High`, `Critical`
