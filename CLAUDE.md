# CLAUDE.md — Instrucciones para Claude Code

## Idioma

Responder **siempre en español**. Léxico peninsular, tuteo natural. Evitar latinismos o variantes latinoamericanas.

---

## Gitflow — OBLIGATORIO

Este proyecto usa gitflow estricto. **NUNCA commitear directamente a `main` ni a `develop`.**

### Flujo completo

```
feature/<nombre>  →  develop  →  release/YYYY.M.P  →  main  →  tag vYYYY.M.P
```

### Pasos detallados

1. Crear `develop` si no existe:
   ```bash
   git checkout -b develop main
   git push -u origin develop
   ```

2. Por cada feature, crear rama desde `develop`:
   ```bash
   git checkout -b feature/<nombre> develop
   ```

3. Hacer commits en la rama feature.

4. Merge a `develop` con `--no-ff`:
   ```bash
   git checkout develop
   git merge --no-ff feature/<nombre>
   git branch -d feature/<nombre>
   ```

5. Cuando las features del ciclo estén listas, crear release:
   ```bash
   git checkout -b release/YYYY.M.P develop
   # bumps de versión, ajustes finales
   git checkout main
   git merge --no-ff release/YYYY.M.P
   git tag -a vYYYY.M.P -m "release vYYYY.M.P"
   git checkout develop
   git merge --no-ff release/YYYY.M.P
   git branch -d release/YYYY.M.P
   ```

### Reglas de naming

- Features: `feature/<kebab-case>` (e.g. `feature/daily-digest`)
- Hotfixes: `hotfix/<descripcion>` desde `main`
- Releases: `release/YYYY.M.P` (año.mes.iteracion)

---

## Antes de cualquier commit

```bash
go build ./...    # DEBE compilar sin errores
go test ./...     # DEBE pasar todos los tests
```

---

## Commits

Usar **Conventional Commits**:

```
feat: descripcion corta
fix: descripcion corta
refactor: descripcion corta
docs: descripcion corta
test: descripcion corta
```

**NO incluir** trailers `Co-Authored-By` en los commits (ver CONTRIBUTING.md).

---

## Arquitectura del proyecto

```
engram/
  cmd/engram/main.go          — CLI: parseo de args, comandos (save, search, digest, export...)
  internal/store/store.go     — SQLite + FTS5: toda la capa de datos
  internal/mcp/mcp.go         — MCP server: herramientas para agentes IA
  internal/server/server.go   — HTTP API REST
  internal/tui/               — Terminal UI (BubbleTea + Lipgloss)
  internal/sync/sync.go       — Sincronizacion git de memoria
  internal/setup/             — Instalacion de plugins por agente
  skills/                     — Skills reutilizables para Claude Code
  plugin/claude-code/         — Plugin oficial para Claude Code
  plugin/opencode/            — Plugin para OpenCode
```

**Stack**: Go 1.25, SQLite (modernc/sqlite), MCP (mark3labs/mcp-go), BubbleTea, Lipgloss.

---

## Memoria Engram — PROTOCOLO OBLIGATORIO

Usar las herramientas MCP de engram proactivamente en cada sesion:

- `mem_search` — buscar contexto ANTES de empezar cualquier tarea
- `mem_save` — guardar decisiones, bugs, patrones y convenciones al terminar cada tarea significativa
- `mem_session_summary` — llamar al final de cada sesion con Goal/Discoveries/Accomplished/Files

**No esperar a que el usuario pida guardar.** Guardar de forma proactiva.

---

## Convenciones de codigo

- Seguir el estilo existente del fichero que se edita
- No añadir comentarios donde la logica sea obvia
- No añadir manejo de errores para escenarios que no pueden ocurrir
- No crear abstracciones para uso unico
- `go build ./...` y `go test ./...` deben pasar siempre antes de commitear
- No usar `--no-verify` ni saltarse hooks

---

## CONTRIBUTING.md (resumen)

- Abrir issue antes de cualquier PR (Issue First)
- PRs enfocados: un cambio logico por PR
- Actualizar docs en el mismo PR si cambia el comportamiento
- No referenciar endpoints o scripts que no existan en el codigo
