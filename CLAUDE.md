# City Roots Farm — Landing Page

## Proyecto

Landing page de ventas para City Roots Farm. Sitio estático single-file (HTML/CSS/JS vanilla) desplegado en GitHub Pages con dominio personalizado `cityroots.farm`.

**Repo GitHub:** `cityroots-app/cityroots-landing`
**Dominio:** `cityroots.farm` (CNAME en el repo)
**Archivo principal:** `index.html` (~1,500 líneas)

---

## Stack técnico

- HTML5 + CSS3 + JavaScript vanilla (sin frameworks, sin build step)
- Google Fonts: Cormorant Garamond (serif), Outfit (body), Josefin Sans (logo)
- Responsive: breakpoint único `@media (max-width: 900px)`
- Animaciones: IntersectionObserver (`.reveal` → `.visible`), parallax, 3D tilt, ripple effects

---

## Estructura del archivo `index.html`

### CSS (líneas 10-510)
- Variables CSS en `:root` (colores, radii, sombras, transiciones)
- Componentes: loader, nav, hero, secciones, modal, cards, botones, formularios
- Media query responsive al final del bloque `<style>`

### HTML Secciones (líneas 510-1090)
1. **Page Loader** — `#pageLoader` con logo animado
2. **Scroll Progress** — `#scrollProgress` barra superior
3. **Nav** — fijo con blur, hamburger mobile, links a secciones
4. **Mobile Nav** — `#mobile-nav` overlay fullscreen
5. **Hero** — logo, h1, stats (40×, 7-14 días, $399), imagen parallax
6. **¿Qué son?** — `#que-son` timeline de crecimiento
7. **Suscripción** — `#suscripcion` timeline 4 semanas, pricing, beneficios
8. **Variedades** — `#variedades` 6 cards (3 activas + 3 próximamente)
9. **Kit de cultivo** — `#kit` incluye, 5 pasos, CTA WhatsApp
10. **Talleres** — `#talleres` evento Residencial Sierra, flyer modal, CTA colonia
11. **Flyer Modal** — `#flyer-overlay` imagen del flyer
12. **Ciencia** — 40× nutrientes (USDA/U. Maryland)
13. **Cómo usarlos** — `#uso` recetas con fotos
14. **Futuro** — 90% agua, 0km huella, 52 cosechas
15. **Contacto** — `#contacto` WhatsApp, email, Instagram
16. **Footer** — links y copyright

### Modal de Suscripción (líneas 1090-1195)
- **Paso 1** `#step1` — formulario (nombre, teléfono, dirección, CP, variedades x4)
- **Paso 2** `#step2` — resumen + botón Mercado Pago + ayuda WhatsApp
- **Confirmación** `#step-confirm` — mensaje de éxito

### JavaScript (líneas 1195-1535)
- Constantes: `PRECIO`, `ZONAS_ENTREGA`, `DIAS_ENTREGA`, `GOOGLE_SHEET_URL`
- Validación CP en tiempo real con auto-asignación de día
- Modal: `openModal()`, `closeModal()`, `showStep1()`, `showStep2()`
- Formulario: `irAlPago()`, `showErr()`, `selDia()`
- Integraciones: `guardarEnSheet()`, `getMsgWA()`, `enviarWA()`, `enviarWAAyuda()`
- Talleres: `abrirFlyer()`, `cerrarFlyer()`
- UX: scroll reveal, parallax, counter animation, ripple, 3D tilt, loader, progress bar

---

## Design system

### Colores
```
--green-deep:   #0f2b0f   (fondos oscuros, nav, footer)
--green-mid:    #2d5a27   (acciones, badges, texto acento)
--green-bright: #4a8c3f   (CTAs, botones primarios)
--green-light:  #a8d5a2   (highlights, hover states)
--green-pale:   #e8f5e3   (fondos de cards, tags)
--amber:        #c87a2e   (badges de evento, accento cálido)
--cream:        #f4f1ea   (fondos alternos de sección)
--warm-white:   #faf9f6   (fondo body)
--charcoal:     #1c1c1a   (texto principal)
--stone:        #6b6b60   (texto secundario)
```

### Tipografía
- **Cormorant Garamond** (serif) — títulos h1-h4, precios grandes
- **Outfit** (sans-serif) — cuerpo, labels, párrafos, botones
- **Josefin Sans** (sans-serif) — logo CITYROOTS.FARM en nav

### Patrones CSS
- Naming: `.section-{nombre}` para secciones, `.btn-{variante}` para botones
- Animaciones: `.reveal` / `.reveal.visible` con IntersectionObserver
- Cards: border-radius 24px, shadow-soft, hover con translateY(-5px)
- Botones: border-radius 50px (pill shape), transiciones 0.3s

---

## Integraciones externas

### WhatsApp
- **Número:** +52 811 778 9996 (formato URL: `528117789996`)
- **Usos:** Kit info, suscripción, talleres, ayuda pago, botón flotante
- **Mensajes:** Templates en `getMsgWA()` y `enviarWAAyuda()`

### Mercado Pago
- **URL:** `https://www.mercadopago.com.mx/subscriptions/checkout?preapproval_plan_id=ae720f53f363402d8fdf280ea736f0f9`
- **Tipo:** Suscripción recurrente $399 MXN/mes
- **Plan ID:** `ae720f53f363402d8fdf280ea736f0f9`

### Google Sheets
- **URL Apps Script:** `https://script.google.com/macros/s/AKfycbw8X6SnOgL-ktTgwNfOtOE1-7BdzDiRnJEsymrKqekb_DnRc0WNhGls6AqjEmTjzXrt/exec`
- **Método:** GET via iframe oculto
- **Campos:** nombre, telefono, direccion, cp, zona, dia, s1-s4, codigo_descuento
- **Sheet:** "Suscripciones_CityRootsFarm"

### Instagram
- **Handle:** @cityroots.farm
- **URL:** `https://www.instagram.com/cityroots.farm/`

---

## Zonas de entrega

### San Pedro Garza García → Miércoles (grupo `sp`)
24 CPs: 66220, 66225, 66250, 66254, 66256, 66257, 66259, 66260, 66263, 66265, 66266, 66267, 66268, 66270, 66273, 66274, 66275, 66278, 66279, 66280, 66285, 66286, 66287, 66290

### Sur de Monterrey → Martes (grupo `sur`)
16 CPs: 64700, 64750, 64830, 64840, 64845, 64858, 64860, 64865, 64890, 64920, 64980, 64985, 64986, 64988, 64989, 64990

---

## Deployment

1. Editar `index.html` localmente en `cityrootsfarm-landing/`
2. Subir a GitHub: `cityroots-app/cityroots-landing` (branch main)
3. GitHub Pages sirve automáticamente con CNAME `cityroots.farm`
4. Cambios se reflejan en 1-2 minutos
5. Las imágenes deben subirse a la carpeta `Images/` del repo

---

## Convenciones

- Todo el copy en **español mexicano**, tono informal pero profesional
- Precios en **MXN**
- El archivo es **single-file** — todo HTML/CSS/JS va en `index.html`
- No agregar dependencias externas (no npm, no CDN frameworks)
- Imágenes en `Images/` con nombres descriptivos PascalCase o snake_case
- Siempre probar cambios en preview antes de subir a producción
