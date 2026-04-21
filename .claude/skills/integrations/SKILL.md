# Skill: Integraciones Externas

## Cuándo usar
Cuando se necesite modificar o debuggear las integraciones con WhatsApp, Mercado Pago o Google Sheets. También para agregar nuevas integraciones.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

---

## WhatsApp

### Datos
- **Número:** +52 811 778 9996
- **Formato URL:** `528117789996`
- **Base:** `https://wa.me/528117789996?text=`

### Puntos de uso en el HTML
1. **Kit de cultivo** (~línea 912) — "¡Hola! Me interesa el Kit de Cultivo..."
2. **Talleres en colonia** (~línea 963) — "¡Hola! Me interesa organizar un taller..."
3. **Contacto** (~línea 1066) — link directo sin mensaje
4. **Botón flotante** (~línea 1530) — link directo sin mensaje
5. **Ayuda en pago** (JS `enviarWAAyuda()`) — "Necesito ayuda para suscribirme..."
6. **Footer** (~línea 1093) — link directo

### Funciones JS

#### `getMsgWA()` (~línea 1388)
```
¡Hola! Quiero suscribirme a los microgreens 🌱
*Nombre:* {nombre}
*Teléfono:* {tel}
*Dirección:* {dir}
*CP:* {cp} ({zona})
*Día de entrega:* {dia}
*Variedades:* S1-S4
*Total:* $399 MXN/mes
```

#### `enviarWAAyuda()` (~línea 1399)
```
¡Hola! Necesito ayuda para suscribirme a los microgreens 🌱
*Nombre:* {nombre}
*Teléfono:* {tel}
*CP:* {cp} ({zona})
¿Me pueden apoyar con el proceso de pago? 🙏
```

---

## Mercado Pago

### Datos
- **URL checkout:** `https://www.mercadopago.com.mx/subscriptions/checkout?preapproval_plan_id=ae720f53f363402d8fdf280ea736f0f9`
- **Plan ID:** `ae720f53f363402d8fdf280ea736f0f9`
- **Tipo:** Suscripción recurrente
- **Precio:** $399 MXN/mes
- **Renovación:** Automática mensual

### Implementación
- Botón `<a>` en Step 2 del modal (~línea 1169)
- `target="_blank"` abre en nueva pestaña
- `onclick="guardarEnSheet()"` guarda datos antes de ir a pago
- Estilo: botón azul (#009ee3) con ícono de tarjeta

### Notas
- No hay integración SDK directa (se usó pero se removió)
- El cobro del descuento se maneja manualmente por ahora
- El `codigo_descuento` se envía a Sheets pero no se aplica en MP

---

## Google Sheets

### Datos
- **Apps Script URL:** `https://script.google.com/macros/s/AKfycbw8X6SnOgL-ktTgwNfOtOE1-7BdzDiRnJEsymrKqekb_DnRc0WNhGls6AqjEmTjzXrt/exec`
- **Sheet:** Suscripciones_CityRootsFarm
- **Método:** GET via iframe oculto (no funciona con fetch/Image por redirect de Apps Script)

### Función JS: `guardarEnSheet()` (~línea 1367)
```javascript
function guardarEnSheet() {
  var params = new URLSearchParams({
    nombre, telefono, direccion, cp, zona, dia, s1, s2, s3, s4, codigo_descuento
  });
  var iframe = document.createElement('iframe');
  iframe.style.display = 'none';
  iframe.src = GOOGLE_SHEET_URL + '?' + params.toString();
  document.body.appendChild(iframe);
  setTimeout(function() { iframe.remove(); }, 5000);
}
```

### Columnas del Sheet
| A | B | C | D | E | F | G | H | I | J | K | L | M |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Fecha | Nombre | Teléfono | Dirección | CP | Zona | Día | Sem1 | Sem2 | Sem3 | Sem4 | Código desc. | Status |

### Apps Script (en Google Sheets)
Tiene dos funciones:
- `doPost(e)` — recibe JSON (actualmente no usado desde la landing)
- `doGet(e)` — recibe query params (usado actualmente via iframe)

### Puntos donde se llama `guardarEnSheet()`
1. Click en botón "Suscribirme y pagar — Mercado Pago" (onclick)
2. Click en "Escríbenos por WhatsApp" en ayuda (`enviarWAAyuda()`)

### Debugging
- Si no registra, verificar que el Apps Script esté deployed como "Cualquier usuario"
- Si cambia la URL del Apps Script, actualizar `GOOGLE_SHEET_URL` en el JS
- Probar manualmente pegando la URL con params en el navegador
- El método iframe funciona desde dominios HTTPS (cityroots.farm) pero puede fallar en file:// local

---

## Instagram

- **Handle:** @cityroots.farm
- **URL:** `https://www.instagram.com/cityroots.farm/`
- **Uso:** Link en sección contacto y footer

## Email
- **Correo:** cityrootsmty@gmail.com
- **Uso:** Link mailto en sección contacto y footer
