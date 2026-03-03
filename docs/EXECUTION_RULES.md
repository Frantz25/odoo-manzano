# Reglas de Ejecución — Odoo Manzano

## 1) Enfoque de trabajo
- **Un issue activo a la vez**.
- No abrir desarrollo fuera del scope del issue activo.
- Si aparece trabajo nuevo, se crea issue y se prioriza; no se ejecuta “en caliente”.

## 2) Flujo de estados (obligatorio)
`todo -> in-progress -> done | blocked`

- `todo`: pendiente, sin ejecución.
- `in-progress`: ejecución activa.
- `blocked`: detenido por dependencia/bloqueo externo.
- `done`: cerrado con evidencia completa.

## 3) Definición de Ready (DoR)
Antes de comenzar un issue debe tener:
- Objetivo claro (1 línea)
- Alcance explícito (qué sí / qué no)
- Criterios de aceptación verificables
- Riesgos/dependencias identificados

## 4) Definición de Done (DoD)
Un issue solo se cierra si incluye:
- Resultado funcional alcanzado
- Evidencia (commit(s), pruebas, salida/log o captura)
- Riesgos remanentes documentados
- Próximo paso recomendado (si aplica)

## 5) Reglas de calidad
- Sin atajos en flujo crítico: si falla validación, se bloquea transición.
- Estado consistente por diseño (no por corrección manual posterior).
- QA mínimo obligatorio antes de cerrar.

## 6) Convención de ramas y commits
- Rama principal: `main`
- Trabajo normal: commits pequeños y trazables.
- Mensajes de commit: `<área>: <cambio>`
  - Ejemplo: `booking: enforce pre-validation before confirm`

## 7) Política de evidencia
Cada cierre de issue debe dejar comentario final con:
1. Qué se implementó
2. Evidencia técnica
3. Cómo probarlo
4. Riesgos o límites conocidos

## 8) Política de no desvío
- No se desarrollan features nuevas si hay P0 abiertos del flujo crítico.
- No se mezclan objetivos de fases distintas en el mismo issue.
