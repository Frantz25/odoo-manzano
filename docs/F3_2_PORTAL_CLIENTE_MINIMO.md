# F3.2 — Portal Cliente (mínimo seguro)

## Objetivo
Habilitar portal para cliente con aceptación de políticas y visibilidad mínima operativa.

## Qué ve el cliente
- Código de reserva
- Fechas (entrada/salida)
- QR de check-in (según estado)
- Estado de reserva

## Qué NO ve el cliente
- Totales internos/comerciales globales
- Participantes globales del evento
- Datos de gestión interna

## Reglas obligatorias
1. Aceptación de políticas requerida antes de confirmación final.
2. Acceso por token/portal seguro al registro propio.
3. Si reserva no está confirmada, QR no puede habilitar ingreso definitivo.

## Criterios de aceptación
- [ ] Cliente puede aceptar políticas desde portal.
- [ ] Portal muestra solo datos mínimos permitidos.
- [ ] Sin exposición de datos internos en vista portal.
- [ ] Flujo consistente con estados booking/order.

## QA mínimo
- Caso A: cliente sin aceptar políticas -> bloquea avance.
- Caso B: cliente acepta -> permite flujo previsto.
- Caso C: reserva cancelada/expirada -> QR inválido en portal.
