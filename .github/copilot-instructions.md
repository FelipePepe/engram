# Copilot Instructions

## Idioma

Responder **siempre en español**. Lexico peninsular. Evitar latinismos.

---

## Gitflow — OBLIGATORIO

**Nunca commitear directamente a `main` ni a `develop`.**

Flujo obligatorio:

```
feature/<nombre>  →  develop  →  release/YYYY.M.P  →  main  →  tag vYYYY.M.P
```

- Una rama `feature/<kebab-case>` por cambio logico, creada desde `develop`.
- Merges siempre con `--no-ff`.
- Hotfixes desde `main` con rama `hotfix/<descripcion>`.
- Releases con rama `release/YYYY.M.P`.

Crear `develop` si no existe:
```bash
git checkout -b develop main && git push -u origin develop
```

---

## Antes de commitear

```bash
go build ./...   # obligatorio — debe compilar sin errores
go test ./...    # obligatorio — deben pasar todos los tests
```

---

## Commits

Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`).
Sin trailers `Co-Authored-By`.

---

## Arquitectura

```
cmd/engram/main.go          — CLI (comandos: save, search, digest, export, tui...)
internal/store/store.go     — SQLite + FTS5: persistencia de observaciones
internal/mcp/mcp.go         — MCP server: herramientas para agentes IA
internal/server/server.go   — HTTP API REST
internal/tui/               — Terminal UI (BubbleTea + Lipgloss)
internal/sync/sync.go       — Sincronizacion de memoria via git
skills/                     — Skills reutilizables
plugin/claude-code/         — Plugin oficial Claude Code
plugin/opencode/            — Plugin OpenCode
```

Stack: Go 1.25, SQLite (modernc/sqlite), mcp-go, BubbleTea, Lipgloss.

---

## Convenciones de codigo

- Seguir el estilo del fichero que se edita.
- No anadir comentarios donde la logica es obvia.
- No crear abstracciones para un solo uso.
- No añadir manejo de errores para casos imposibles.
- No usar `--no-verify` ni saltarse hooks de git.

---

## Contribuciones (ver CONTRIBUTING.md)

- Issue First: abrir issue antes de cualquier PR.
- PR enfocado: un cambio logico por PR.
- Actualizar docs en el mismo PR si cambia el comportamiento.
