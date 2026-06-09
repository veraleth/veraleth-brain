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

## DEC-004 · 2026-06-09 · Backend — Rust + Axum
**Contexto:** Se necesitaba elegir el stack del backend de veraleth-api. Se priorizó robustez, rendimiento y control sobre velocidad de desarrollo inicial.
**Decisión:** Backend en **Rust + Axum** con **SQLx** como driver de base de datos (async, compile-time checked) y runtime **Tokio**.
**Consecuencias:**
- veraleth-api será un proyecto Rust con Axum
- SQLx maneja queries a PostgreSQL (Supabase por debajo)
- El scraper (veraleth-scraper) se mantiene en Python + Scrapy — son sistemas independientes
- Mayor curva inicial pero binario único, memoria mínima y errores detectados en compilación

## DEC-005 · 2026-06-09 · Base de datos — PostgreSQL vía Supabase
**Contexto:** Se necesitaba decidir dónde correr PostgreSQL para el MVP.
**Decisión:** Usar **Supabase** como PostgreSQL managed para el MVP. Supabase Auth queda disponible pero la decisión de usarlo se toma en DEC-006.
**Consecuencias:**
- veraleth-db contiene schemas y migraciones (SQLx migrate)
- No se levanta instancia propia de PostgreSQL en Hetzner por ahora
- Si el proyecto escala, se puede migrar a instancia propia sin cambiar el código (mismo driver)

## DEC-006 · 2026-06-09 · Auth de usuarios — Supabase Auth
**Contexto:** Veraleth necesita cuentas de usuario para feedback ciudadano y reportes. Se evaluaron JWT propio, Supabase Auth y Clerk.
**Decisión:** Usar **Supabase Auth** como sistema de autenticación. El backend Axum valida el JWT emitido por Supabase en cada request sin llamar a Supabase en cada operación.
**Consecuencias:**
- Supabase maneja: registro, email verificado, reset de contraseña, refresh tokens
- veraleth-api valida JWT de Supabase en middleware Axum — sin lógica de auth propia
- Costo cero en MVP — migrable si el proyecto escala
- Supabase Auth y Supabase DB corren bajo el mismo proyecto Supabase

## DEC-007 · 2026-06-09 · Primera fuente de scraping — Wong (wong.pe)
**Contexto:** Se necesitaba elegir el primer supermercado peruano para el MVP del scraper de precios.
**Decisión:** Arrancar con **wong.pe** (Cencosud). DOM estable basado en React, categorías bien estructuradas.
**Consecuencias:**
- veraleth-scraper implementa primero el spider de Wong
- Por ser Cencosud, el mismo spider es adaptable a metro.pe con cambios mínimos — dos fuentes al precio de una
- Tottus y Plaza Vea quedan como Fase 2 del scraper
