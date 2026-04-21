# Skill: Mobile & Responsive Design

## Cuándo usar
Para mejorar la experiencia móvil, ajustar breakpoints, arreglar problemas de layout en pantallas pequeñas, o mejorar touch targets y usabilidad mobile.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html` — Media query en líneas ~471-507

---

## Breakpoint actual

Un solo breakpoint: `@media (max-width: 900px)`

### Cambios que aplica (línea ~471)
```css
@media (max-width: 900px) {
  /* Hero */
  .hero { grid-template-columns: 1fr; }
  .hero::before, .hero::after { display: none; }  /* Oculta decoraciones */
  .hero-right { height: 50vh; }
  .hero-left { padding: 7rem 1.5rem 3rem; }
  .hero h1 { font-size: 2.6rem; }
  .hero-stat-row { gap: 1.5rem; }

  /* Secciones */
  .section-intro, .section-care, .section-cta, .kit-grid { grid-template-columns: 1fr; gap: 3rem; padding: 4rem 1.5rem; }
  .section-benefits, .section-uses, .section-science, .section-future, .section-productos, .section-kit { padding: 4rem 1.5rem; }
  .section-suscripcion { padding: 4rem 1.5rem; }

  /* Nav */
  nav { padding: 1rem 1.5rem; }
  nav.scrolled { padding: 0.7rem 1.5rem; }
  nav ul { display: none; }         /* Oculta menú desktop */
  .hamburger { display: flex; }     /* Muestra hamburger */

  /* Grids → 1 columna */
  .benefits-grid, .uses-grid, .future-grid, .productos-grid { grid-template-columns: 1fr; }
  .kit-steps { grid-template-columns: repeat(2,1fr); }  /* 2 col en mobile */
  .sus-timeline { grid-template-columns: repeat(2,1fr); gap: 2rem; }
  .sus-timeline::before { display: none; }
  .sus-benefits { grid-template-columns: 1fr; }
  .growth-timeline { grid-template-columns: repeat(2,1fr); }

  /* Flexbox → column */
  .sus-pricing { flex-direction: column; align-items: flex-start; gap: 1.5rem; }
  .sus-pricing-cta { align-items: flex-start; width: 100%; }
  .sus-header { flex-direction: column; align-items: flex-start; gap: 1rem; }
  .benefits-header, .productos-header { flex-direction: column; align-items: flex-start; gap: 1rem; }
  .section-science { flex-direction: column; gap: 2rem; }
  .section-science::after { display: none; }

  /* Text alignment */
  .sus-header p, .benefits-header p, .productos-header p { text-align: left; }

  /* Cards */
  .use-card.featured { grid-row: span 1; min-height: 260px; }

  /* Footer */
  footer { padding: 2.5rem 1.5rem; }

  /* Talleres */
  .section-talleres { padding: 4rem 1.5rem; }
  .talleres-grid { grid-template-columns: 1fr; }
  .temas-list { grid-template-columns: 1fr; }
  .taller-cta { flex-direction: column; text-align: center; padding: 2rem 1.5rem; }
  .taller-cta-img { width: 160px; height: 160px; order: -1; }
  .taller-cta-text { text-align: center; }
}
```

---

## Mobile Nav (línea ~464)
- `.mobile-nav-overlay` — fullscreen overlay con backdrop-filter blur(20px)
- Links en Cormorant Garamond 1.8rem, centrados verticalmente
- CTA "Ordenar ahora" como botón pill verde al final
- Toggle: `toggleMobileNav()` — toggle `.open` class + hamburger `.active`
- Body overflow hidden cuando está abierto

---

## Mejoras recomendadas

### Breakpoints adicionales
```css
/* Tablet landscape */
@media (max-width: 1200px) {
  /* Reducir padding de secciones de 5rem a 3rem */
  /* Grids de 3 col → 2 col */
}

/* Mobile small */
@media (max-width: 480px) {
  .hero h1 { font-size: 2.2rem; }
  .hero-left { padding: 6rem 1rem 2rem; }
  .kit-steps { grid-template-columns: 1fr; }  /* 1 col en muy pequeño */
  .sus-timeline { grid-template-columns: 1fr; }
  .growth-timeline { grid-template-columns: 1fr; }
}
```

### Touch targets
- **Mínimo 44×44px** para todos los elementos clickeables (estándar Apple/Google)
- Botones actuales: ✅ OK (padding 0.9rem 2rem)
- Nav links mobile: ✅ OK (1.8rem font con gap 2rem)
- Hamburger: ⚠️ Verificar que tenga al menos 44px de hit area
- Selects de variedad: ⚠️ Pueden ser pequeños en mobile
- Links del footer: ⚠️ Agregar padding para mayor área de toque

### Modal en mobile
- El modal usa `max-width: 520px` — funciona bien en mobile
- **Mejora:** En pantallas < 480px, hacer modal fullscreen:
  ```css
  @media (max-width: 480px) {
    .modal-box { max-width: 100%; max-height: 100vh; border-radius: 0; margin: 0; }
  }
  ```

### Scroll horizontal prevention
- Ya tiene `overflow-x: hidden` en body ✅
- Verificar que imágenes grandes no causen overflow

### Tipografía mobile
- Hero h1: 2.6rem (fijo) — Considerar `clamp()` para transición suave
- Body text: sin cambio en mobile — 300 weight puede verse fino en pantallas pequeñas, subir a 400

### Performance mobile
1. **Desactivar 3D tilt** en mobile (solo funciona con mouse):
   ```css
   @media (hover: none) {
     .producto-card, .benefit-card { transform: none !important; }
   }
   ```
2. **Reducir parallax** o desactivar en mobile (consume batería)
3. **Lazy load más agresivo** en mobile

### Safe areas (notch/dynamic island)
```css
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
}
.floating-wa {
  bottom: calc(2rem + env(safe-area-inset-bottom));
  right: calc(2rem + env(safe-area-inset-right));
}
```

---

## Testing mobile

### Desde Chrome DevTools
1. `Cmd+Opt+I` → Toggle device toolbar (`Cmd+Shift+M`)
2. Probar: iPhone SE (375px), iPhone 14 (390px), iPad (768px)

### Desde preview tool
Usar `preview_resize` con dimensiones mobile: `390×844` (iPhone), `768×1024` (iPad)

## Checklist mobile
- [ ] Todos los touch targets ≥ 44×44px
- [ ] Modal fullscreen en < 480px
- [ ] Desactivar 3D tilt en touch devices
- [ ] Verificar que todo el texto sea legible sin zoom
- [ ] Probar formulario de suscripción completo en mobile
- [ ] Verificar flyer modal cierre fácil en mobile
- [ ] Safe area insets para iPhones con notch
- [ ] Font-weight 400 en mobile para legibilidad
