# Protocolo Claude Code — Veraleth

## Rutina de inicio de sesión

Leer SIEMPRE en este orden:

1. `veraleth-brain/CURRENT/STATUS.md` — estado actual (ATENDER ALERTA Supabase NO configurado)
2. `veraleth-brain/CURRENT/DECISIONS.md` — decisiones vigentes (ver DEC-008 Supabase)
3. Solo si el tema lo requiere: leer archivo específico (ARCHITECTURE, ROADMAP, etc.)

**ALERTA BLOQUEANTE:** Supabase NO configurado — resolver antes de tareas DB/migraciones.

## Reglas fundamentales

1. **Scrapers frágiles — HTML cambia frecuente:**
   - Validar estructura HTML antes de deploy
   - Supermercados (Tottus/Metro/Plaza Vea) cambian sin aviso

2. **Rate limiting obligatorio:**
   - Máximo 1 request/3s por scraper
   - NO saturar servers supermercados

3. **Alerta bloqueante: Supabase NO configurado:**
   - Preguntar a Rodhan antes de tareas DB/migraciones
   - Ver DEC-008 para estado actual

4. **Toda decisión técnica → nueva entrada DEC-XXX:**
   - Agregar a `veraleth-brain/CURRENT/DECISIONS.md`

5. **GROWTH.md y DESIGN.md solo Perplexity + Rodhan:**
   - Claude Code NO modifica salvo instrucción explícita

## División de responsabilidades

**Perplexity:** investiga, debate, genera prompts, escribe en veraleth-brain (docs). Nunca escribe código.

**Claude Code:** ejecuta prompts, escribe código, actualiza STATUS.md al finalizar. Nunca decide sin prompt.

**Rodhan:** decisiones de negocio, aprobación arquitectura, dirección producto.

## Protocolo diagnóstico antes de fix

Antes de generar cualquier prompt de cambio, evaluar:

1. ¿STATUS.md fue actualizado en las últimas 24h? Si no → pedir diagnóstico
2. ¿La tarea involucra scrapers o cambios HTML? → validar estructura actual primero
3. ¿El cambio afecta DB y Supabase NO está configurado? → bloquear hasta resolver

## Regla de oro

veraleth-brain es la única fuente de verdad.
Perplexity debate y genera prompts. Claude Code ejecuta; nunca decide.

---

## Rutina de cierre de sesión — OBLIGATORIO

1. Actualizar STATUS.md (estado actual, commits recientes, alertas, próximos pasos)
2. git add . && git commit -m "docs: end-of-session sync $(date +%Y-%m-%d)"
3. git push (brain + repos de código que tuvieron cambios)

⚠️ Sin push al final de sesión, Perplexity lee datos desactualizados en GitHub
   la próxima vez que use MCP para tomar contexto del proyecto.
