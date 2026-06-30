---
type: reference
title: "GROWTH — Estrategia de Crecimiento"
description: "Estrategia de adquisición, canales y hitos comerciales. Mantenido por Perplexity + Rodhan. Claude Code NO modifica este archivo salvo instrucción explícita."
tags: [growth, comercial, adquisicion, estrategia]
timestamp: 2026-06-30
---

# GROWTH — Estrategia de Crecimiento

> Mantenido por Perplexity + Rodhan. Claude Code NO modifica este archivo salvo instrucción explícita.
> Última actualización: 2026-06-30 | 📝 Provisional

## Estado comercial actual
- Fase: Desarrollo — 0 usuarios activos | Scraping DIGEMID/INDECOPI/supermercados
- Modelo: Gratis MVP, post-validación premium (alertas ilimitadas + historial extendido)
- Stack: Python Scrapy + Next.js + PostgreSQL (Supabase)

## Cliente objetivo
- **Primario**: padres familia conscientes (verifican ingredientes antes comprar)
- **Secundario**: personas condiciones médicas (verifican concentración medicamentos)
- **Uso**: búsqueda por nombre producto o código barras, alertas Telegram

## Top 3 canales de adquisición
1. **SEO**: "ingredientes dañinos [producto]", "alerta DIGEMID [medicamento]"
2. **TikTok/Reels**: videos "qué tiene REALMENTE [producto popular]" (ingredientes ocultos)
3. **Grupos Facebook padres**: "madres conscientes Lima", "alimentación saludable niños"

## Diferenciador clave
- Agregador único: DIGEMID + INDECOPI + supermercados en una búsqueda
- Alertas proactivas: alza precio anómala >15% o lote medicamento retirado
- Fuente verificable: cada dato tiene source_url + date_scraped (transparencia)

## Hitos de crecimiento
- [ ] Scraper Wong/Plaza Vea operativo (primeros 1000 productos)
- [ ] Landing pública con búsqueda productos + alertas
- [ ] Primera alerta DIGEMID detectada y notificada (Telegram)
- [ ] 100 usuarios suscritos alertas → validar engagement
- [ ] Viral: 1 video TikTok >10k views mostrando ingrediente oculto
