# E2.2 — Confirmación Atómica (Blueprint técnico)

## Objetivo
Asegurar que una confirmación final sea todo-o-nada:
- si falla cualquier validación, no se confirma nada;
- si todo cumple, order y booking quedan consistentes en una sola transición lógica.

## Regla central
No puede existir `sale.order` en `sale` con `booking.state != confirmed`.

---

## Secuencia técnica propuesta

1. **Pre-validaciones (obligatorias, en este orden):**
   - políticas aceptadas
   - disponibilidad vigente
   - abono mínimo requerido
2. **Lock funcional del registro** (evitar carrera de doble confirmación concurrente).
3. **Confirmación comercial** (`sale.order`)
4. **Confirmación operativa** (`booking -> confirmed`)
5. **Post-acciones** (QR definitivo, eventos/comms, trazabilidad)

Si falla cualquier paso 1–4: `raise` y rollback.

---

## Contratos de métodos (diseño)

### `validate_for_final_confirmation(order)`
Retorna OK o lanza `UserError`.

### `confirm_booking_atomic(order)`
Orquesta la transición completa.

### `sync_order_booking_states(order, booking)`
Garantiza mapeo final coherente.

---

## Errores funcionales esperados
- Sin políticas aceptadas
- Sin disponibilidad
- Abono insuficiente
- Booking inexistente o inválido

Todos deben bloquear confirmación sin efectos parciales.

---

## Criterios de aceptación E2.2
- [ ] No hay confirmación parcial en caso de error.
- [ ] Si confirma, order+booking quedan consistentes.
- [ ] Existe trazabilidad de bloqueo/confirmación.
- [ ] QA negativo/positivo reproducible.

---

## Casos QA mínimos

### Caso negativo A: sin abono
Resultado esperado: bloquea, order permanece sin confirmar.

### Caso negativo B: sin disponibilidad
Resultado esperado: bloquea, sin cambios parciales.

### Caso positivo: todo OK
Resultado esperado: order confirmada + booking confirmado + QR/estado coherente.
