# DECISIONS — Veraleth

Registro de todas las decisiones arquitectónicas y de negocio del proyecto.
Formato: DEC-XXX · Fecha · Título · Contexto · Decisión · Consecuencias

---

## DEC-001 · 2026-06-09 · Nombre del proyecto
**Contexto:** Se necesitaba un nombre para la plataforma cívica de vigilancia de alimentos, precios y medicamentos en Perú.
**Decisión:** El proyecto se llama **Veraleth**. Raíz: *vera* (verdad) + *leth* (fluir) — transparencia y flujo de información.
**Consecuencias:** Dominio objetivo: veraleth.pe. Org GitHub: github.com/veraleth.

## DEC-002 · 2026-06-09 · Estructura de repositorios
**Contexto:** Se necesitaba definir cómo organizar el código bajo la org veraleth.
**Decisión:** Cinco repos: veraleth-brain (docs), veraleth-api (backend), veraleth-web (frontend), veraleth-scraper (scraping), veraleth-db (base de datos).
**Consecuencias:** Cada repo tiene responsabilidad única. veraleth-brain es la fuente de verdad — nunca contiene código ejecutable.

## DEC-003 · 2026-06-09 · División de responsabilidades IA
**Contexto:** El proyecto usa dos IAs con roles distintos.
**Decisión:** Perplexity debate y genera prompts. Claude Code documenta y construye. El repo es el registro de todo lo acordado.
**Consecuencias:** Claude Code nunca decide — solo ejecuta prompts generados por Perplexity + Rodhan.
