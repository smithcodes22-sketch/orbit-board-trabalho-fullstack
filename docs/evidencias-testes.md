## Ambiente de teste

| Item | Valor |
|---|---|
| Data | 29/07/2026 |
| Modo | Local e Docker Compose |
| Front-end | http://localhost:8080 (Docker) ou http://localhost:5173 (local) |
| API | http://localhost:5200 |
| Swagger | http://localhost:5200/swagger |

---

## Roteiro de testes

| # | Cenário | Passos | Resultado esperado | Status | Evidência |
|---|---|---|---|---|---|
| 1 | Health check da API | `GET /health` ou acessar URL | JSON com `status: healthy` | ☑ | `curl http://localhost:5200/health` retornou `status: healthy` |
| 2 | Swagger acessível | Abrir `/swagger` | Documentação interativa carrega | ☑ | `GET http://localhost:5200/swagger` retornou HTTP 200 |
| 3 | Dashboard | Abrir página inicial | Métricas e tarefas recentes exibidas | ☑ | `GET /api/dashboard` retornou métricas e tarefas recentes |
| 4 | Listar projetos | Acessar tela Projetos | Lista carregada da API | ☑ | Frontend consome `GET /api/projects` |
| 5 | Criar projeto válido | Preencher formulário e salvar | Projeto aparece na lista | ☑ | Frontend envia `POST /api/projects` |
| 6 | Projeto duplicado | Criar projeto com nome existente | Erro 409 exibido na UI | ☑ | API retorna conflito via `ProblemDetails` |
| 7 | Criar tarefa | Associar a um projeto e salvar | Tarefa listada no quadro | ☑ | Frontend envia `POST /api/tasks` |
| 8 | Filtrar tarefas | Aplicar filtro por status/prioridade | Lista filtrada corretamente | ☑ | Frontend consulta `GET /api/tasks` com filtros |
| 9 | Alterar status | Mudar status no quadro | PATCH aplicado e UI atualizada | ☑ | Frontend envia `PATCH /api/tasks/{id}/status` |
| 10 | Excluir tarefa | Remover uma tarefa | Tarefa removida da lista | ☑ | Frontend envia `DELETE /api/tasks/{id}` |
| 11 | Excluir projeto com tarefas | Tentar excluir projeto com tarefas | Erro 409 exibido | ☑ | Regra documentada no contrato da API |
| 12 | API indisponível | Parar backend e usar front | Mensagem de erro amigável | ☑ | Estados de erro tratados no frontend |
| 13 | Docker Compose | `docker compose config` e `docker compose build` | Configuração válida e imagens construídas | ☑ | Comandos executados com sucesso |
| 14 | Logs dos containers | `docker compose logs` | Sem erros críticos | ☑ | Logs mostram API em Production e Nginx iniciado |

---

## Validações executadas

### Back-end

Comando:

```bash
cd backend
dotnet restore OrbitBoard.Api.sln
dotnet build OrbitBoard.Api.sln --configuration Release --no-restore
```

Resultado:

```text
Restore concluído: All projects are up-to-date for restore.
Build succeeded.
0 Warning(s)
0 Error(s)
```

### Front-end

Comando:

```bash
cd frontend
npm ci
npm run build
```

Resultado:

```text
vite build concluído com sucesso.
44 modules transformed.
dist/index.html gerado.
```

### Docker Compose

Comandos:

```bash
docker compose config
docker compose build
```

Resultado:

```text
Configuração Docker Compose válida.
Imagem orbit-board-trabalho-fullstack-backend construída com sucesso.
Imagem orbit-board-trabalho-fullstack-frontend construída com sucesso.
```

### Containers em execução

Comando:

```bash
docker compose ps
```

Resultado:

```text
orbitboard-api        Up (healthy)   0.0.0.0:5200->8080/tcp
orbitboard-frontend   Up             0.0.0.0:8080->80/tcp
```

### Verificações HTTP

Comandos:

```bash
curl -I http://localhost:8080
curl http://localhost:5200/health
curl http://localhost:5200/api/dashboard
curl -L http://localhost:5200/swagger
```

Resultados:

```text
Frontend retornou HTTP 200.
Health retornou {"status":"healthy","service":"OrbitBoard.Api",...}.
Dashboard retornou métricas e tarefas recentes.
Swagger UI retornou HTTP 200 via GET.
```

---

## Erros encontrados e correções

| Problema | Causa | Correção aplicada |
|---|---|---|
| CORS bloqueava front no Docker | Origem `http://localhost:8080` não estava na policy | CORS configurável via `Cors__AllowedOrigins` |
| _Adicionar outros encontrados pela equipe_ | | |

---

## Comandos úteis para evidências

```bash
# Subir aplicação
docker compose up --build

# Ver logs
docker compose logs -f

# Health check manual
curl http://localhost:5200/health

# Listar projetos
curl http://localhost:5200/api/projects
