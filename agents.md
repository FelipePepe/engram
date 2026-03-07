# agents.md — Instrucciones para agentes IA

## Idioma

Responder **siempre en español**. Lexico peninsular. Evitar latinismos.

---

## Reglas criticas antes de tocar git

1. **Nunca commitear directamente a `main` ni a `develop`.**
2. Crear siempre una rama `feature/<nombre>` desde `develop`.
3. Compilar y pasar tests antes de cualquier commit:
   ```bash
   go build ./...
   go test ./...
   ```

## Gitflow

```
feature/<nombre>  →  develop  →  release/YYYY.M.P  →  main  →  tag vYYYY.M.P
```

- Merges con `--no-ff` siempre.
- Naming: `feature/<kebab-case>`, `hotfix/<descripcion>`, `release/YYYY.M.P`.

---

## Arquitectura

- `cmd/engram/main.go` — CLI y punto de entrada
- `internal/store/store.go` — SQLite + FTS5, toda la persistencia
- `internal/mcp/mcp.go` — Herramientas MCP para agentes IA
- `internal/server/server.go` — HTTP API REST
- `internal/tui/` — Terminal UI (BubbleTea)
- `internal/sync/sync.go` — Sync de memoria via git
- `skills/` — Skills reutilizables para agentes

---

## Memoria

Usar las herramientas MCP `mem_search`, `mem_save` y `mem_session_summary` de forma proactiva:

- Buscar en memoria al inicio de cada tarea (`mem_search`)
- Guardar decisiones y descubrimientos al terminar cada tarea (`mem_save`)
- Guardar resumen al final de cada sesion (`mem_session_summary`)

---

## Commits

Conventional Commits. Sin trailers `Co-Authored-By`.

---

## Codigo

- No anadir comentarios donde la logica sea obvia
- No crear abstracciones para un solo uso
- No anadir manejo de errores para casos imposibles
- Seguir el estilo del fichero existente
