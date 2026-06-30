---
type: reference
title: "Visión del Producto"
description: "Documento fundacional — qué es Veraleth, para quién, y hacia dónde va. Claude Code debe leer este archivo antes de proponer cambios de arquitectura o nuevas funcionalidades."
tags: [vision, product, estrategia]
timestamp: 2026-06-29
---

# VISION.md — Veraleth

## ¿Qué es Veraleth?

Plataforma de monitoreo de seguridad alimentaria y medicamentos en Perú. Combina scraping automatizado de fuentes oficiales (DIGEMID, INDECOPI) + supermercados + feedback ciudadano para alertar sobre ingredientes dañinos, concentraciones incorrectas, y alzas de precios anómalas.

**Valor diferencial:** Agrega datos dispersos de entidades públicas y privadas en una base de datos unificada, con alertas proactivas y comunidad verificada que reporta incidencias en tiempo real.

## El problema que resuelve

**Problema:** Consumidores peruanos enfrentan:
- Información sobre seguridad alimentaria dispersa (DIGEMID, INDECOPI, portales de supermercados)
- Sin alertas proactivas sobre lotes de medicamentos defectuosos
- No hay historial de precios accesible (alzas anómalas pasan desapercibidas)
- Etiquetas de alimentos difíciles de interpretar (ingredientes dañinos no evidentes)

**Solución:** Veraleth permite:
1. Buscar producto por nombre o código de barras
2. Ver alertas de DIGEMID (medicamentos con concentración incorrecta, lotes retirados)
3. Detectar ingredientes dañinos en alimentos (colorantes artificiales, aceite de palma, aditivos)
4. Historial de precios con detección de alzas anómalas
5. Reportes ciudadanos verificados (usuarios con cuenta autenticada)

## Para quién

**Usuarios primarios:**
1. **Consumidores conscientes** — verifican ingredientes y precios antes de comprar
2. **Padres de familia** — buscan alimentos sin aditivos dañinos para niños
3. **Personas con condiciones médicas** — verifican concentración real de medicamentos

**Contexto de uso:**
- Web-first (Next.js en Vercel)
- Búsqueda por nombre de producto o código de barras
- Alertas vía Telegram (opcional, suscripción a productos específicos)
- Comunidad con cuentas verificadas (email) para reportes

> ⚠️ Pendiente — completar con Rodhan: modelo de monetización (ads, premium, donaciones), fuentes de datos iniciales prioritarias, zonas geográficas (Lima first o nacional)

## Arquitectura de producto

**Stack tecnológico (inicial):**
- **Scraping:** Python + Scrapy (cron en GitHub Actions o servidor mínimo)
- **Fuentes de datos:** DIGEMID, INDECOPI, supermercados peruanos (Wong, Plaza Vea, Metro)
- **Backend:** Node.js o Python FastAPI (DEC-004 pendiente) + PostgreSQL (Supabase)
- **Auth:** Cuentas propias con email verificado
- **Frontend:** Next.js (Vercel)
- **Secrets:** Infisical
- **Alertas:** Telegram
- **Infraestructura:** Hetzner + Ubuntu 24.04

**Módulos del producto:**
1. **Alimentos** — ingredientes dañinos, colorantes artificiales, aceite de palma, aditivos por producto
2. **Medicamentos** — concentraciones declaradas vs. reportadas, alertas por lote (fuente: DIGEMID)
3. **Precios** — historial por producto, detección de alzas anómalas, scraping periódico
4. **Comunidad** — cuentas de usuarios verificados, feedback ciudadano, sistema de reportes

**Flujo principal:**
```
Scrapers automáticos (DIGEMID, INDECOPI, supermercados)
      ↓
Ingesta a PostgreSQL (productos, alertas, precios)
      ↓
Usuario busca producto por nombre/código
      ↓
Frontend muestra alertas + ingredientes + historial precios
      ↓
Usuario reporta incidencia (cuenta verificada)
      ↓
Alerta Telegram a suscriptores de ese producto
```

> ⚠️ Pendiente — completar con Rodhan: frecuencia de scraping, threshold para alertas de precio, moderación de reportes ciudadanos

## Principios que Claude Code debe respetar

### 1. Fuentes verificables en cada dato
- Todo dato de alerta o ingrediente tiene `source_url` + `date_scraped`
- NO mostrar alertas sin fuente oficial (DIGEMID, INDECOPI)
- Reportes ciudadanos marcados claramente como "no verificado oficialmente"

### 2. Scraping periódico, no on-demand
- Scrapers corren en cron (diario o semanal según fuente)
- NO scraping en tiempo real (evita sobrecarga de fuentes)
- Cache de resultados en PostgreSQL

### 3. Comunidad verificada, no anónima
- Reportes requieren cuenta con email verificado
- NO permitir reportes anónimos (evita spam/desinformación)
- Sistema de moderación para reportes flagged

### 4. Backend pendiente de decisión (DEC-004)
- Opciones: Node.js o Python FastAPI
- NO implementar lógica de negocio hasta que DEC-004 se resuelva
- Ver [DECISIONS.md](DECISIONS.md#dec-004)

### 5. Alertas vía Telegram, no email masivo
- Telegram como canal principal de alertas
- Email solo para verificación de cuenta
- NO spam — usuario se suscribe solo a productos que le interesan

## Estado actual

**Fase:** Desarrollo MVP

**Componentes operativos:**
- ✅ `veraleth-api` — estructura base (Rust + Axum, /health)
- ✅ `veraleth-db` — schema base (products, price_history, reports)
- ✅ `veraleth-scraper` — estructura base (WongSpider placeholder)
- ✅ `veraleth-web` — desplegado en Vercel (página placeholder pública)

**Bloqueado hasta configuración Supabase (usuario Rodhan):**
- ❌ Aplicar migraciones (`sqlx migrate run`)
- ❌ Conectar veraleth-api a PostgreSQL real
- ❌ Implementar spider Wong con inserción real a DB
- ❌ Primer endpoint real: GET /products

**Decisiones pendientes:**
- DEC-004: Backend — Node.js vs Python FastAPI

**Próximo paso (cuando Supabase listo):**
1. Agregar `DATABASE_URL` y `SUPABASE_JWT_SECRET` a Infisical
2. Ejecutar migraciones desde veraleth-db
3. Implementar spider Wong real (selectores DOM wong.pe)
4. Primer endpoint real en veraleth-api: GET /products

Ver [STATUS.md](STATUS.md) y [ROADMAP.md](ROADMAP.md) para estado detallado.
