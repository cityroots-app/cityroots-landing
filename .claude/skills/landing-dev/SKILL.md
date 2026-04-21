# Skill: Landing Page Development

## Cuándo usar
Cuando se necesite modificar, agregar o mejorar secciones del landing page de City Roots Farm. Incluye: nuevas secciones HTML, estilos CSS, animaciones, responsive design, y mejoras de UX.

## Archivo principal
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

## Estructura del archivo
- **Líneas 1-10:** Head, meta tags, fonts
- **Líneas 10-510:** CSS embebido (variables, componentes, responsive)
- **Líneas 510-1090:** HTML secciones del body
- **Líneas 1090-1195:** Modal de suscripción
- **Líneas 1195-1535:** JavaScript

## Design system

### Variables CSS (`:root`)
```css
--green-deep: #0f2b0f;    --green-mid: #2d5a27;
--green-bright: #4a8c3f;   --green-light: #a8d5a2;
--green-pale: #e8f5e3;     --amber: #c87a2e;
--cream: #f4f1ea;          --warm-white: #faf9f6;
--charcoal: #1c1c1a;       --stone: #6b6b60;
--radius-lg: 24px;         --radius-md: 16px;
--shadow-soft: 0 8px 40px rgba(15,43,15,0.08);
--shadow-hover: 0 20px 60px rgba(15,43,15,0.14);
--transition-smooth: cubic-bezier(0.22, 1, 0.36, 1);
```

### Tipografía
- `Cormorant Garamond` → títulos (h1-h4)
- `Outfit` → cuerpo, botones, labels
- `Josefin Sans` → logo nav

### Secciones existentes (en orden)
1. `#que-son` — `.section-intro`
2. `#suscripcion` — `.section-suscripcion`
3. (sin id) — `.section-benefits`
4. `#variedades` — `.section-productos`
5. `#kit` — `.section-kit`
6. `#talleres` — `.section-talleres`
7. (sin id) — `.section-science`
8. `#uso` — `.section-uses`
9. (sin id) — `.section-future`
10. `#contacto` — `.section-cta`

### Nav links
```html
<ul>
  <li><a href="#que-son">¿Qué son?</a></li>
  <li><a href="#suscripcion">Suscripción</a></li>
  <li><a href="#variedades">Variedades</a></li>
  <li><a href="#kit">Kit de cultivo</a></li>
  <li><a href="#talleres">Talleres</a></li>
  <li><a class="nav-cta" onclick="openModal()">Ordenar ahora</a></li>
</ul>
```
También existe mobile nav `#mobile-nav` que debe actualizarse en paralelo.

### Convenciones para nuevas secciones
1. Agregar CSS antes del comentario `/* FUTURO */`
2. Agregar responsive en el `@media (max-width: 900px)` al final
3. HTML entre la sección anterior y la siguiente
4. Usar clase `.reveal` para scroll animations
5. Seguir naming: `.section-{nombre}` para la sección
6. Si tiene enlace en nav, usar `id="{nombre}"` y agregar `<a href="#{nombre}">` en nav Y mobile-nav

### Animaciones disponibles
- `.reveal` → fadeUp al hacer scroll (IntersectionObserver)
- `.reveal-left` → fadeIn desde la izquierda
- `.reveal-scale` → scale + fadeIn
- Parallax en hero image
- 3D tilt en cards (`.producto-card`, `.benefit-card`, `.future-card`, `.kit-step`)
- Ripple en botones (`.btn-primary`, `.btn-full.green`, `.nav-cta`)

### Imágenes
Carpeta: `Images/` (con I mayúscula)
Referencia en HTML: `src="Images/nombre.png"`
Formatos: PNG preferido
