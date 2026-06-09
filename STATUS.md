# STATUS — Veraleth

## Estado actual
**Fase:** Desarrollo MVP
**Fecha de última actualización:** 2026-06-09
**Responsable:** Rodhan

## Repos
| Repo | Estado |
|------|--------|
| veraleth-brain | ✅ Activo — fuente de verdad |
| veraleth-api | ✅ Estructura base (Rust + Axum, /health) |
| veraleth-db | ✅ Schema base (products, price_history, reports) |
| veraleth-scraper | ✅ Estructura base (WongSpider placeholder) |
| veraleth-web | ✅ Desplegado en Vercel — página placeholder pública |

## ¿Qué está en curso?
- veraleth-web público en Vercel
- Supabase pendiente de configuración manual por Rodhan

## Bloqueado hasta que Rodhan configure Supabase
- Aplicar migraciones (sqlx migrate run)
- Conectar veraleth-api a PostgreSQL real
- Implementar spider Wong con inserción real a DB

## Próximo paso (cuando Supabase esté listo)
1. Agregar DATABASE_URL y SUPABASE_JWT_SECRET a Infisical
2. Ejecutar migraciones desde veraleth-db
3. Implementar spider Wong real (selectores DOM wong.pe)
4. Primer endpoint real en veraleth-api: GET /products

## Alertas para Perplexity
⚠️ Supabase no configurado — preguntar a Rodhan si ya creó el proyecto antes de continuar con tareas de DB o scraping.
