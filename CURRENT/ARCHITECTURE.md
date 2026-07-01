---
type: concept
title: "Architecture"
description: "Este documento describe el stack y la arquitectura del sistema."
tags: [architecture]
timestamp: 2026-06-20
---

# ARCHITECTURE — Veraleth

> Este documento describe el stack y la arquitectura del sistema.
> Sujeto a cambios — ver [DECISIONS.md](DECISIONS.md) para el historial de decisiones vigentes.

## Stack (inicial)

| Capa | Tecnología |
|------|-----------|
| Scraping | Python + Scrapy · cron económico (GitHub Actions o servidor mínimo) |
| Fuentes de datos | DIGEMID · INDECOPI · supermercados peruanos (DOM estable) |
| Backend | Node.js o Python FastAPI (a decidir — ver próxima decisión) · PostgreSQL + Supabase |
| Auth usuarios | Cuentas propias con email verificado |
| Frontend | Next.js · Vercel |
| Secrets | Infisical |
| Alertas internas | Telegram |
| Infraestructura | Hetzner · Ubuntu 24.04 |

## Módulos del producto

- **Alimentos:** ingredientes dañinos, colorantes artificiales, aceite de palma, aditivos por producto
- **Medicamentos:** concentraciones declaradas vs. reportadas, alertas por lote (fuente: DIGEMID)
- **Precios:** historial por producto, detección de alzas anómalas, scraping periódico
- **Comunidad:** cuentas de usuarios verificados, feedback ciudadano, sistema de reportes

## Decisiones pendientes
- [DEC-004](DECISIONS.md#dec-004) (pendiente): elección de backend — Node.js vs Python FastAPI
