# E2.2 — Pasos de Implementación (ejecución)

## Objetivo de este bloque
Implementar confirmación atómica sin estados parciales.

## Paso 1 — Contratos funcionales
Definir métodos con responsabilidad única:
- `validate_for_final_confirmation(order)`
- `confirm_booking_atomic(order)`
- `sync_order_booking_states(order, booking)`

## Paso 2 — Orden estricto de validación
1. Políticas aceptadas
2. Disponibilidad vigente
3. Abono mínimo

Si falla cualquiera: `UserError` y terminar.

## Paso 3 — Confirmación todo-o-nada
- Ejecutar confirmación comercial y operativa en una única transición lógica.
- Prohibido dejar `sale.order= sale` con booking no confirmado.

## Paso 4 — Trazabilidad
- En éxito: mensaje de confirmación atómica.
- En bloqueo: mensaje funcional claro (sin confirmar nada).

## Paso 5 — QA mínimo obligatorio
### Negativo A
Sin abono mínimo -> bloquea, no confirma.

### Negativo B
Sin disponibilidad -> bloquea, no confirma.

### Positivo
Con políticas + disponibilidad + abono -> confirma order+booking.

## Invariante final
`order.state = sale` implica `booking.state = confirmed`.
