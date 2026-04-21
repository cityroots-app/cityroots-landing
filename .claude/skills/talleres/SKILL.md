# Skill: Talleres (Workshops)

## Cuándo usar
Cuando se necesite agregar, modificar o actualizar información de talleres: nuevo evento, cambiar fecha/ubicación, actualizar flyer, agregar más talleres, o modificar la sección de talleres en la landing.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

## Sección HTML (~línea 921)
ID: `#talleres` | Clase: `.section-talleres`

### Estructura
```
section#talleres.section-talleres
  .talleres-header.reveal (título + descripción)
  h3 "Próximos talleres"
  .talleres-grid (2 columnas)
    .taller-card.reveal (card del evento con imagen)
    .taller-temas.reveal (panel "¿Qué aprenderás?")
  .taller-cta.reveal (CTA "taller en tu colonia")
```

### Evento actual
- **Título:** Siembra, cultiva y cosecha tus propios microgreens
- **Fecha:** Sábado 25 de abril · 10:00 AM
- **Ubicación:** Palapa Parque del Canal · Residencial de la Sierra
- **Precio:** $150 MXN (kit incluido)
- **Badge:** "Evento privado · Residencial de la Sierra"
- **Cupo:** Limitado

### Temas del taller (panel derecho)
1. 🌱 Kit de cultivo completo
2. 🎓 Plática interactiva (nutrición, superalimentos)
3. 🧑‍🌾 Conoce a tu productor
4. 📖 Guía paso a paso (instrucciones + QR)

### Tags de temas
Microgreens, Nutrientes, Producción de alimentos, Agroquímicos, Know your farmer, Alimentación real

### CTA inferior
- **Texto:** "¿Te gustaría hacer un taller en tu colonia?"
- **Botón WhatsApp:** enlace a `wa.me/528117789996` con mensaje pre-llenado
- **Imagen:** `Images/Taller.png`

## Flyer modal (~línea 975)
```html
<div class="flyer-overlay" id="flyer-overlay">
  <div class="flyer-box">
    <button class="flyer-close" onclick="cerrarFlyer()">✕</button>
    <img src="Images/flyer_taller_25abril.png">
  </div>
</div>
```

### Funciones JS
- `abrirFlyer()` — muestra el modal (clase `.show`)
- `cerrarFlyer()` — oculta el modal
- Click en `.taller-card` dispara `abrirFlyer()`

## Imágenes
- `Images/Taller.png` — Foto de niños en taller (2.2MB)
- `Images/flyer_taller_25abril.png` — Flyer del evento (450KB)

## Cómo agregar un nuevo taller

1. En `.talleres-grid`, agregar otra `.taller-card` con la info del nuevo evento
2. Si hay flyer, agregar imagen a `Images/` y crear nuevo modal o reutilizar `#flyer-overlay`
3. Actualizar el badge y la fecha
4. Para múltiples talleres, considerar cambiar grid a scroll horizontal o accordion

## Cómo actualizar el taller existente

1. Buscar `section#talleres` en el HTML
2. Modificar: fecha (`.taller-date`), título (`h3`), descripción (`p`), detalles (`.taller-detail` spans)
3. Si cambia el flyer: reemplazar `Images/flyer_taller_25abril.png` y actualizar `src` en `#flyer-overlay`
4. Si cambia la foto: reemplazar `Images/Taller.png`

## CSS específico
- `.section-talleres` — padding 6rem 5rem, max-width 1200px
- `.talleres-grid` — grid 2 columnas, gap 2.5rem
- `.taller-card` — white bg, radius-lg, shadow, hover translateY(-4px)
- `.taller-badge` — amber bg, pill shape, uppercase, absolute sobre imagen
- `.taller-img` — height 280px, object-fit cover
- `.taller-temas` — white card con lista de temas
- `.taller-cta` — green-deep gradient bg, flex con imagen
- `.flyer-overlay` — fixed fullscreen con backdrop blur
- **Responsive (900px):** grid 1 columna, temas-list 1 columna, CTA column con imagen arriba
