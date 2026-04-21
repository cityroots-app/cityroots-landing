# Skill: Design System & Visual Identity

## Cuándo usar
Para mejorar la coherencia visual, refinar la paleta de colores, mejorar tipografía, espaciado, jerarquía visual, y asegurar que toda la landing se vea premium y profesional.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html` — CSS en líneas 10-510

---

## Paleta actual y mejoras sugeridas

### Colores actuales
```css
--green-deep:   #0f2b0f   /* Nav, footer, fondos oscuros */
--green-mid:    #2d5a27   /* CTAs, acciones */
--green-bright: #4a8c3f   /* Acentos, highlights */
--green-light:  #a8d5a2   /* Fondos suaves */
--green-pale:   #e8f5e3   /* Cards, tags */
--amber:        #c87a2e   /* Badges, accento cálido */
--amber-light:  #e8b44c   /* Accento secundario */
--cream:        #f4f1ea   /* Fondos alternos */
--warm-white:   #faf9f6   /* Body background */
--charcoal:     #1c1c1a   /* Texto principal */
--stone:        #6b6b60   /* Texto secundario */
```

### Variables que faltan (agregar para más control)
```css
/* Tipografía — escala modular (ratio 1.25) */
--text-xs:    0.72rem;    /* Labels, badges, small text */
--text-sm:    0.85rem;    /* Nav links, captions */
--text-base:  1rem;       /* Body text */
--text-lg:    1.15rem;    /* Lead paragraphs */
--text-xl:    1.4rem;     /* Section subtitles */
--text-2xl:   clamp(1.8rem, 3vw, 2.2rem);   /* h3 */
--text-3xl:   clamp(2.2rem, 4vw, 3.2rem);   /* h2 */
--text-4xl:   clamp(3.2rem, 5.5vw, 5rem);   /* h1 hero */

/* Spacing — escala 8px base */
--space-xs:   0.5rem;     /* 8px */
--space-sm:   1rem;       /* 16px */
--space-md:   1.5rem;     /* 24px */
--space-lg:   2.5rem;     /* 40px */
--space-xl:   4rem;       /* 64px */
--space-2xl:  6rem;       /* 96px — padding de sección */
--space-3xl:  8rem;       /* 128px — padding hero */

/* Sombras — sistema de elevación */
--shadow-xs:    0 1px 3px rgba(15,43,15,0.04);
--shadow-sm:    0 4px 12px rgba(15,43,15,0.06);
--shadow-md:    0 8px 40px rgba(15,43,15,0.08);   /* = shadow-soft actual */
--shadow-lg:    0 20px 60px rgba(15,43,15,0.14);   /* = shadow-hover actual */
--shadow-glow:  0 0 40px rgba(74,140,63,0.15);     /* Glow verde para CTAs */
```

## Tipografía — Mejoras

### Jerarquía actual
- h1: Cormorant Garamond, clamp(3.2rem, 5.5vw, 5rem), weight 400
- h2: Cormorant Garamond, variable por sección
- Body: Outfit, weight 300
- Labels: Outfit, 0.72rem, uppercase, letter-spacing 0.15em

### Mejoras recomendadas
1. **Subir peso del body de 300 a 400** — 300 puede verse demasiado delgado en pantallas de baja resolución
2. **Agregar `font-display: swap`** en el link de Google Fonts para evitar FOIT
3. **Line-height consistente** — usar 1.5 para body, 1.15 para headings, 1.7 para párrafos descriptivos
4. **Letter-spacing negativo en h1/h2** — `-0.02em` a `-0.03em` para aspecto editorial premium
5. **Italic strategy** — usar `<em>` solo en una palabra clave por heading para crear énfasis sin abusar

### Escala tipográfica recomendada (mobile → desktop)
```
h1 hero:    2.6rem → 5rem       (Cormorant, 400, -0.02em)
h2 section: 2rem → 3.2rem       (Cormorant, 400)
h3 card:    1.3rem → 1.6rem     (Cormorant, 600)
h4 small:   0.9rem → 1rem       (Outfit, 500)
body:       0.9rem → 1rem       (Outfit, 400, line-height 1.7)
small:      0.78rem → 0.85rem   (Outfit, 300)
label:      0.68rem → 0.72rem   (Outfit, 500, uppercase, 0.15em)
```

## Layout & Espaciado

### Secciones — patrón actual
```css
.section-* { padding: 8rem 5rem; }
@media (max-width: 900px) { padding: 4rem 1.5rem; }
```

### Mejoras recomendadas
1. **Max-width container** — agregar `max-width: 1200px; margin: 0 auto;` a todas las secciones para evitar contenido demasiado ancho en pantallas >1440px
2. **Ritmo vertical** — separar secciones con espaciado consistente (8rem desktop / 4rem mobile)
3. **Grid gaps** — usar gap consistente: 2rem para grids de cards, 3rem para grids de contenido
4. **Content width** — párrafos descriptivos nunca más de 600px de ancho (`max-width: 600px`)

## Color — Mejoras

### Contraste
- `--stone` (#6b6b60) sobre `--warm-white` (#faf9f6) → ratio 4.8:1 ✅ (AA)
- `--green-mid` (#2d5a27) sobre white → ratio 8.2:1 ✅ (AAA)
- `--green-bright` (#4a8c3f) sobre white → ratio 4.1:1 ⚠️ (AA mínimo, subir a #3d7a35 para AAA)

### Gradientes premium sugeridos
```css
/* Hero overlay sutil */
background: linear-gradient(135deg, rgba(15,43,15,0.03) 0%, rgba(200,122,46,0.03) 100%);

/* CTA botón premium */
background: linear-gradient(135deg, var(--green-mid) 0%, var(--green-bright) 100%);

/* Section divider sutil */
border-image: linear-gradient(90deg, transparent, var(--green-light), transparent) 1;
```

## Componentes — Estándares

### Botones
```css
/* Primario */
padding: 0.9rem 2rem;
border-radius: 50px;
font-weight: 500;
font-size: 0.92rem;
letter-spacing: 0.02em;
box-shadow: 0 4px 20px rgba(45,90,39,0.25);
transition: all 0.4s var(--transition-smooth);

/* Hover: translateY(-3px) + shadow más grande */
/* Active: translateY(0) + shadow reducida */
```

### Cards
```css
background: white;
border-radius: var(--radius-lg);  /* 24px */
box-shadow: var(--shadow-soft);
transition: all 0.5s var(--transition-smooth);
/* Hover: translateY(-5px) + shadow-hover */
```

### Inputs del formulario
```css
border: 1.5px solid var(--border);
border-radius: 12px;
padding: 0.85rem 1rem;
font-size: 0.88rem;
transition: border-color 0.3s, box-shadow 0.3s;
/* Focus: border-color: var(--green-mid); box-shadow: 0 0 0 3px rgba(45,90,39,0.08); */
```

## Textura de fondo (ya implementada)
```css
body::after {
  /* Noise texture overlay sutil (opacity 0.025) */
  /* Da textura orgánica premium al fondo */
}
```

## Checklist de mejora visual
- [ ] Agregar variables de spacing y tipografía al `:root`
- [ ] Max-width 1200px en todas las secciones
- [ ] Subir font-weight body a 400
- [ ] Verificar contraste WCAG AA en todos los textos
- [ ] Agregar `font-display: swap` al import de Google Fonts
- [ ] Gradiente sutil en botones primarios
- [ ] Hover states consistentes en todos los elementos clickeables
- [ ] Focus states visibles para accesibilidad (outline o box-shadow)
- [ ] Separadores entre secciones más sutiles (gradiente línea o espacio)
- [ ] Aspecto editorial: max-width en párrafos, line-height generoso
