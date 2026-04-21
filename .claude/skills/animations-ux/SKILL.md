# Skill: Animaciones & Micro-interacciones

## Cuándo usar
Para mejorar, agregar o debuggear animaciones, transiciones, efectos hover, scroll-reveal, parallax, o cualquier micro-interacción del sitio.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

---

## Inventario de animaciones actuales

### 1. Page Loader (CSS + JS, línea ~35 y ~1479)
- Logo pulsa (`loaderPulse`: scale 1→1.08, opacity 1→0.7)
- Barra desliza (`loaderSlide`: translateX -100%→350%)
- Se oculta 600ms después de `window.load` con clase `.hidden` (opacity + visibility transition 0.6s)

### 2. Scroll Progress Bar (JS, línea ~1484)
- `#scrollProgress`: barra verde fija en top, width = % de scroll
- CSS: `position: fixed; top: 0; height: 3px; background: var(--green-bright); z-index: 1000; transition: width 0.1s;`

### 3. Scroll Reveal (JS, línea ~1434)
- **IntersectionObserver** con `threshold: 0.08`, `rootMargin: '0px 0px -50px 0px'`
- Clases: `.reveal`, `.reveal-left`, `.reveal-scale`
- Al entrar en viewport → agrega `.visible` con **staggered delay** (100ms × índice entre siblings)
- CSS transitions en `.reveal`:
  ```css
  .reveal { opacity: 0; transform: translateY(40px); transition: all 0.8s var(--transition-smooth); }
  .reveal.visible { opacity: 1; transform: translateY(0); }
  .reveal-left { opacity: 0; transform: translateX(-40px); transition: all 0.8s var(--transition-smooth); }
  .reveal-left.visible { opacity: 1; transform: translateX(0); }
  .reveal-scale { opacity: 0; transform: scale(0.92); transition: all 0.8s var(--transition-smooth); }
  .reveal-scale.visible { opacity: 1; transform: scale(1); }
  ```

### 4. Parallax Hero (JS, línea ~1447 y ~1518)
- Imagen hero se mueve con scroll: `translateY(scrollY * 0.15)` y `scale(1.05) translateY(scrollY * 0.08)`
- Solo activo cuando `scrollY < innerHeight`
- Usa `{ passive: true }` para performance

### 5. Counter Animation (JS, línea ~1455)
- IntersectionObserver con `threshold: 0.5`
- Selectors: `.fc-num, .float-num, .d-num`
- Cuenta de 0 al número objetivo en incrementos de `target/40` cada 30ms
- Se ejecuta una sola vez (unobserve después)

### 6. Nav Scroll Effect (JS, línea ~1416)
- Agrega `.scrolled` al nav cuando `scrollY > 60`
- CSS: `nav.scrolled` reduce padding, agrega `backdrop-filter: blur(20px)`, cambia fondo a semitransparente

### 7. Button Ripple Effect (JS, línea ~1490)
- Click en `.btn-primary, .btn-full.green, .nav-cta, .btn-mp-pago`
- Crea `<span class="btn-ripple">` posicionado en el punto del click
- CSS: scale 0→4 con opacity 1→0, duración 0.6s
- Se auto-remueve después de 600ms

### 8. 3D Card Tilt (JS, línea ~1503)
- Aplica a: `.producto-card:not(.proximamente), .benefit-card, .future-card, .use-card:not(.featured), .kit-step`
- En `mousemove`: `perspective(800px) rotateY(x*6deg) rotateX(-y*6deg) translateY(-4px)`
- En `mouseleave`: reset con transition 0.5s

### 9. Floating WhatsApp (CSS, línea ~44)
- `floatWA`: translateY 0→-6px→0 (3s ease-in-out infinite)
- `waPulseRing`: scale 1→1.4 con opacity 0.6→0 (2s infinite)
- Hover: scale(1.1) translateY(-2px) + shadow más grande

### 10. Hover States generales (CSS)
- Cards: `translateY(-5px)` + shadow-hover (0.5s transition)
- Botones: `translateY(-3px)` + shadow más grande
- Nav links: color transition 0.3s
- Hamburger: spans rotan para formar X

---

## Mejoras recomendadas

### Performance
1. **Reducir listeners de scroll** — Hay 3 listeners de scroll separados (nav, parallax, progress). Unificar en uno solo con `requestAnimationFrame`
2. **will-change** — Agregar `will-change: transform` a elementos con animaciones frecuentes (hero-img, cards)
3. **prefers-reduced-motion** — Agregar media query para usuarios que prefieren menos movimiento:
   ```css
   @media (prefers-reduced-motion: reduce) {
     *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
     .reveal, .reveal-left, .reveal-scale { opacity: 1; transform: none; }
   }
   ```

### Micro-interacciones nuevas
1. **Focus ring animado** en inputs del formulario (box-shadow pulse suave)
2. **Skeleton loading** para imágenes pesadas mientras cargan
3. **Smooth scroll-to-section** con offset para el nav fijo
4. **Hover en imágenes de recetas** — zoom sutil (scale 1.05) con transition
5. **Toast notification** después de guardar en Google Sheets (confirmación visual)
6. **Typing indicator** en el campo de CP mientras valida

### Transiciones entre pasos del modal
- Actualmente: `display: none/block` (sin animación)
- Mejora: slide horizontal con opacity, o fade-in con translateY

## Cómo agregar una nueva animación

1. **CSS-only**: Agregar `@keyframes` y clase en el bloque `<style>`
2. **Scroll-triggered**: Agregar clase `.reveal` (o `.reveal-left`, `.reveal-scale`) al elemento HTML
3. **JS custom**: Agregar al bloque `<script>` antes del cierre `</script>` (línea ~1527)
4. **Hover**: Usar CSS `transition` + `:hover` pseudo-class
