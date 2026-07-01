---
type: reference
title: "Decisions"
description: "# Decisions — Veraleth documentation"
tags: [decisions]
timestamp: 2026-06-20
---

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
**Decisión:** Usar **Supabase** como PostgreSQL managed para el MVP. Supabase Auth queda disponible pero la decisión de usarlo se toma en [DEC-006](DECISIONS.md#dec-006).
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

## DEC-008 · 2026-06-09 · Supabase pendiente de configuración
**Contexto:** Se necesita un proyecto Supabase real para conectar veraleth-api y correr migraciones.
**Decisión:** Postergado — Rodhan creará el proyecto en supabase.com manualmente. Una vez disponible: agregar DATABASE_URL y SUPABASE_JWT_SECRET a Infisical y ejecutar sqlx migrate run.
**Consecuencias:**
- Spider Wong y conexión real de veraleth-api quedan bloqueados hasta tener Supabase activo
- veraleth-web puede desplegarse en Vercel de forma independiente (no depende de Supabase)
- Próxima sesión técnica arranca con: "Supabase listo → aplicar migraciones → spider Wong"

## DEC-009 · 2026-06-09 · Gestor de paquetes JS — pnpm obligatorio
**Contexto:** Se necesita estandarizar el gestor de paquetes para todos los proyectos JS/TS del ecosistema Veraleth.
**Decisión:** Usar **pnpm** exclusivamente. npm queda prohibido en todos los repos.
**Consecuencias:**
- Todos los repos JS/TS usan pnpm install, pnpm add, pnpm run
- veraleth-web debe tener pnpm-lock.yaml — si existe package-lock.json, eliminarlo
- Vercel CLI se instala con pnpm add -g vercel
- Claude Code nunca usa npm en ningún prompt de Veraleth

## DEC-010 · 2026-06-20 · Referencias cruzadas navegables en documentación
**Contexto:** Las referencias entre documentos del repo aparecían como texto plano o negritas no clickeables, lo que impedía navegación directa en GitHub y resolución automática por Perplexity vía MCP.
**Decisión:** Toda referencia a otra decisión o documento dentro de veraleth-brain debe usar el patrón `[DEC-XXX](DECISIONS.md#dec-xxx)` con ancla GitHub. Referencias sin ancla solo se permiten cuando la ancla es incierta.
**Consecuencias:**
- Claude Code audita referencias sueltas en cada sesión antes de agregar contenido nuevo
- Perplexity puede resolver referencias automáticamente al leer el repo vía MCP
- Script de validación pendiente: `validate-refs.sh` que detecte referencias sueltas antes del commit (Fase 2)
