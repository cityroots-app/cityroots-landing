# Skill: Subscription Flow

## Cuándo usar
Cuando se necesite modificar el flujo de suscripción: modal de registro, formulario, validaciones, paso de pago, resumen de pedido, mensajes de WhatsApp, o integración con Mercado Pago.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

## Estructura del modal

### HTML del modal (líneas ~1090-1195)
```
#modal-overlay (flex overlay, click-outside cierra)
  .modal-box (#modal-box)
    .modal-header (logo + título + close btn)
    .step-indicators (#ind1, #ind2)
    #step1 — Formulario
    #step2 — Resumen + pago
    #step-confirm — Confirmación
```

### Campos del formulario (Step 1)
| ID | Campo | Validación |
|----|-------|-----------|
| `f-nombre` | Nombre completo | No vacío |
| `f-telefono` | Teléfono/WhatsApp | Mínimo 10 dígitos |
| `f-direccion` | Dirección de entrega | No vacío |
| `f-cp` | Código postal | 5 dígitos + existir en ZONAS_ENTREGA |
| `f-dia` | Día de entrega | Auto-asignado por CP (hidden) |
| `f-s1` a `f-s4` | Variedad por semana | Debe seleccionarse |

### Variedades disponibles en selects
- 🌶 Spicy Mix (activa)
- 🟢 Chícharo (activa)
- 🔵 Brócoli — Próximamente (disabled)
- 🌻 Girasol (activa)
- 🔴 Rábano (activa)
- 🍈 Melón — Próximamente (disabled)

### Día de entrega
Auto-asignado según código postal:
- San Pedro (grupo `sp`) → **Miércoles**
- Sur Monterrey (grupo `sur`) → **Martes**
El campo `f-dia` se llena automáticamente al validar CP. No hay selector manual.

### Step 2 — Pago
- **Botón principal:** "Suscribirme y pagar — Mercado Pago" → abre link externo + `guardarEnSheet()`
- **Ayuda:** "Escríbenos por WhatsApp" → `enviarWAAyuda()` con mensaje de soporte
- **Volver:** `volverPaso1()` regresa a Step 1

## Funciones JavaScript clave

### `irAlPago()` (~línea 1328)
Valida todos los campos, construye objeto `pedido`, genera HTML del resumen, llama `showStep2()`.

### `guardarEnSheet()` (~línea 1367)
Envía datos a Google Sheets via iframe oculto con URLSearchParams. Campos: nombre, telefono, direccion, cp, zona, dia, s1-s4, codigo_descuento.

### `getMsgWA()` (~línea 1388)
Template del mensaje de WhatsApp con datos del pedido. Formato con negritas (*campo*) y emojis.

### `enviarWAAyuda()` (~línea 1399)
Mensaje de ayuda más corto, solo nombre/teléfono/CP. Llama `guardarEnSheet()` también.

### Objeto `pedido`
```javascript
pedido = { nombre, tel, dir, cp, zona, dia, s1, s2, s3, s4 }
```

## Mercado Pago
- **URL:** `https://www.mercadopago.com.mx/subscriptions/checkout?preapproval_plan_id=ae720f53f363402d8fdf280ea736f0f9`
- **Precio:** $399 MXN/mes recurrente
- Se abre en nueva pestaña (`target="_blank"`)

## Google Sheets
- **URL:** `https://script.google.com/macros/s/AKfycbw8X6SnOgL-ktTgwNfOtOE1-7BdzDiRnJEsymrKqekb_DnRc0WNhGls6AqjEmTjzXrt/exec`
- **Método:** GET via iframe oculto (evita CORS)
- **Sheet:** Suscripciones_CityRootsFarm
- **Columnas:** Fecha | Nombre | Teléfono | Dirección | CP | Zona | Día entrega | Sem 1-4 | Código desc. | Status

## WhatsApp
- **Número:** 528117789996
- **Base URL:** `https://wa.me/528117789996?text=`
