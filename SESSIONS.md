# SESSIONS — Veraleth

Registro cronológico de sesiones de trabajo.

---

## SESSION-001 · 2026-06-09
**Participantes:** Rodhan + Perplexity
**Tema:** Fundación del proyecto
**Acordado:**
- Nombre: Veraleth
- Org GitHub: github.com/veraleth
- Repos definidos: brain, api, web, scraper, db
- Stack inicial definido (backend pendiente de decisión)
- División de responsabilidades: Perplexity debate, Claude Code ejecuta

**Ejecutado por Claude Code:**
- Inicialización de veraleth-brain
- Creación de [STATUS.md](STATUS.md), [DECISIONS.md](DECISIONS.md), [ARCHITECTURE.md](ARCHITECTURE.md), [ROADMAP.md](ROADMAP.md), [SESSIONS.md](SESSIONS.md)

**Próxima sesión:** Decidir backend (Node.js vs FastAPI) + inicializar repos restantes

---

## SESSION-002 · 2026-06-09
**Participantes:** Rodhan + Perplexity
**Tema:** Definición de stack y creación de repos de código

**Acordado:**
- [DEC-004](DECISIONS.md#dec-004): Backend Rust + Axum + SQLx
- [DEC-005](DECISIONS.md#dec-005): PostgreSQL vía Supabase
- [DEC-006](DECISIONS.md#dec-006): Auth con Supabase Auth, JWT validado en Axum
- [DEC-007](DECISIONS.md#dec-007): wong.pe como primera fuente de scraping

**Ejecutado por Claude Code:**
- veraleth-api inicializado (Rust + Axum, endpoint /health)
- veraleth-db inicializado (schema base: products, product_ingredients, price_history, reports)
- veraleth-scraper inicializado (Scrapy + WongSpider base)
- veraleth-web inicializado (Next.js + Tailwind, página placeholder)

**Próxima sesión:**
- Implementar spider Wong real (selectores DOM de wong.pe)
- Conectar veraleth-api a Supabase (DATABASE_URL real)
- Desplegar veraleth-web en Vercel

---

## SESSION-003 · 2026-06-09
**Participantes:** Rodhan + Perplexity
**Tema:** Deploy veraleth-web + documentación de bloqueo Supabase + estandarización pnpm

**Acordado:**
- Supabase queda como pendiente manual de Rodhan ([DEC-008](DECISIONS.md#dec-008))
- pnpm es el gestor de paquetes obligatorio en todos los repos JS/TS ([DEC-009](DECISIONS.md#dec-009))
- veraleth-web se despliega en Vercel como página pública placeholder
- El spider Wong y la conexión real de la API quedan bloqueados hasta Supabase

**Ejecutado por Claude Code:**
- veraleth-web migrado a pnpm y desplegado en Vercel
- [DEC-008](DECISIONS.md#dec-008) y [DEC-009](DECISIONS.md#dec-009) registrados en [DECISIONS.md](DECISIONS.md)
- STATUS.md actualizado con bloqueo explícito y alerta para Perplexity

**Próxima sesión:**
- Rodhan confirma Supabase listo → aplicar migraciones → spider Wong → GET /products

---

## SESSION-004 · 2026-06-20
**Participantes:** Rodhan + Perplexity
**Tema:** Implementación de sistema de referencias cruzadas navegables

**Acordado:**
- [DEC-010](DECISIONS.md#dec-010): todas las referencias entre docs deben ser links con ancla GitHub

**Ejecutado por Claude Code:**
- Auditoría de referencias sueltas en todos los archivos .md del repo
- Corrección de [DECISIONS.md](DECISIONS.md), [STATUS.md](STATUS.md), [SESSIONS.md](SESSIONS.md), [ARCHITECTURE.md](ARCHITECTURE.md), [ROADMAP.md](ROADMAP.md)
- [DEC-010](DECISIONS.md#dec-010) registrado
- [STATUS.md](STATUS.md) actualizado

**Próxima sesión:**
- Rodhan confirma Supabase listo → aplicar migraciones → spider Wong → GET /products
