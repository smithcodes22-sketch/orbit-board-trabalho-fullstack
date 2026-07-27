## Ambiente de teste

| Item | Valor |
|---|---|
| Data | _preencher_ |
| Modo | Local / Docker Compose |
| Front-end | http://localhost:8080 (Docker) ou http://localhost:5173 (local) |
| API | http://localhost:5200 |
| Swagger | http://localhost:5200/swagger |

---

## Roteiro de testes

| # | Cenário | Passos | Resultado esperado | Status | Evidência |
|---|---|---|---|---|---|
| 1 | Health check da API | `GET /health` ou acessar URL | JSON com `status: healthy` | ☐ | |
| 2 | Swagger acessível | Abrir `/swagger` | Documentação interativa carrega | ☐ | |
| 3 | Dashboard | Abrir página inicial | Métricas e tarefas recentes exibidas | ☐ | |
| 4 | Listar projetos | Acessar tela Projetos | Lista carregada da API | ☐ | |
| 5 | Criar projeto válido | Preencher formulário e salvar | Projeto aparece na lista | ☐ | |
| 6 | Projeto duplicado | Criar projeto com nome existente | Erro 409 exibido na UI | ☐ | |
| 7 | Criar tarefa | Associar a um projeto e salvar | Tarefa listada no quadro | ☐ | |
| 8 | Filtrar tarefas | Aplicar filtro por status/prioridade | Lista filtrada corretamente | ☐ | |
| 9 | Alterar status | Mudar status no quadro | PATCH aplicado e UI atualizada | ☐ | |
| 10 | Excluir tarefa | Remover uma tarefa | Tarefa removida da lista | ☐ | |
| 11 | Excluir projeto com tarefas | Tentar excluir projeto com tarefas | Erro 409 exibido | ☐ | |
| 12 | API indisponível | Parar backend e usar front | Mensagem de erro amigável | ☐ | |
| 13 | Docker Compose | `docker compose up --build` | Ambos containers sobem saudáveis | ☐ | |
| 14 | Logs dos containers | `docker compose logs` | Sem erros críticos | ☐ | |

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