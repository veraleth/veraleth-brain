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

## Convenciones OKF — obligatorio en todo archivo MD del brain

### Frontmatter obligatorio
Todo archivo creado o modificado en CURRENT/ debe tener:
```yaml
---
type: concept | runbook | reference | log
title: "Título del documento"
description: "Descripción breve (1-2 líneas)"
tags: [tag1, tag2]   # máximo 5
timestamp: YYYY-MM-DD
# Campos opcionales:
volatile: true        # solo STATUS.md
resource: ../ruta/    # apunta a código/CAD/assets fuera del brain (DEC-002)
refs:                 # apunta a otros docs del brain (DEC-004)
  - OTRO-DOC.md
  - DECISIONS.md
---
```

### Cuándo usar resource: vs refs:
- `resource:` → el doc describe un artefacto externo al brain (código, CAD, config)
  Ejemplo: ARCHITECTURE.md que documenta src/modulo/ → resource: ../../src/modulo/
- `refs:` → el doc depende o referencia otros docs del brain
  Ejemplo: REQUIREMENTS.md que referencia GROWTH.md → refs: [GROWTH.md, ROADMAP.md]

### Cross-references entre documentos
- Siempre usar links Markdown con anclas: [DEC-014](DECISIONS.md#dec-014)
- Nunca duplicar contenido de otro doc — solo referenciar
- Decisiones superadas: marcar [SUPERSEDED por DEC-XXX] al inicio de la sección
- Antes de crear contenido nuevo, verificar que no existe ya en otro archivo del brain

### Numeración DEC
- Formato: DEC-001, DEC-002... (secuencial por proyecto)
- Scope en commit: feat(DEC-XXX): o docs(DEC-XXX):
- Consultar DECISIONS.md para saber el próximo número disponible

---

## Rutina de cierre de sesión — OBLIGATORIO

1. Actualizar STATUS.md (estado actual, commits recientes, alertas, próximos pasos)
2. git add . && git commit -m "docs: end-of-session sync $(date +%Y-%m-%d)"
3. git push (brain + repos de código que tuvieron cambios)

⚠️ Sin push al final de sesión, Perplexity lee datos desactualizados en GitHub
   la próxima vez que use MCP para tomar contexto del proyecto.
