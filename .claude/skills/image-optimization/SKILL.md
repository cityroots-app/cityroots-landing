# Skill: Optimización de Imágenes

## Cuándo usar
Para mejorar tiempos de carga, reducir peso de imágenes, implementar lazy loading, o resolver problemas de calidad/display de imágenes.

## Archivos
- **HTML:** `/Users/geminismkt/claude/cityrootsfarm-landing/index.html`
- **Imágenes:** `/Users/geminismkt/claude/cityrootsfarm-landing/Images/`

---

## Inventario de imágenes actual

| Archivo | Tamaño | Uso | Prioridad |
|---------|--------|-----|-----------|
| Principal_.png | 2.8 MB | Hero background | Crítica (above the fold) |
| 00_Sandwich.png | 2.7 MB | Receta card | Baja (below fold) |
| 00_Smoothie.png | 2.5 MB | Receta card | Baja |
| 00_Produccion.png | 2.4 MB | Sección intro | Media |
| 00_Ensalada.png | 2.4 MB | Receta card | Baja |
| 00_Hotcakes.png | 2.3 MB | Receta card | Baja |
| 00_Guacamole.png | 2.3 MB | Receta card | Baja |
| Taller.png | 2.3 MB | Talleres CTA | Baja |
| 00_KitdeCultivo.png | 1.9 MB | Kit section | Media |
| Portada.png | 1.7 MB | Suscripción | Media |
| Flyer_Taller.png | 450 KB | Modal flyer | Baja |
| LOGO_VERDE.png | 70 KB | — | — |
| LOGO_VERDE_nobg.png | 53 KB | — | — |
| ISOTIPO_VERDE.png | 36 KB | — | — |
| ISOTIPO_VERDE_nobg.png | 30 KB | Loader + Nav | Alta |

**Total: ~23.5 MB** — Muy pesado para una landing page.

---

## Problemas actuales

### 1. Peso excesivo
- Las 7 imágenes de producto/recetas pesan 2-2.8 MB cada una (PNG sin comprimir)
- **Meta ideal:** < 200 KB por imagen, total del sitio < 3 MB
- **Solución:** Convertir a WebP con fallback JPG, resize a máximo 800px de ancho

### 2. No hay lazy loading
- Todas las imágenes cargan al inicio, incluyendo las que están muy abajo
- **Solución:** Agregar `loading="lazy"` a todas las imágenes excepto hero y loader logo

### 3. No hay dimensiones explícitas
- Las imágenes no tienen `width` y `height` en el HTML → causa CLS (Cumulative Layout Shift)
- **Solución:** Agregar atributos `width` y `height` que reflejen el aspect ratio

### 4. Hero image sin preload
- `Principal_.png` (2.8 MB) es critical path pero no tiene preload
- **Solución:** `<link rel="preload" as="image" href="Images/Principal_.png">`

---

## Optimización recomendada

### Paso 1 — Comprimir con sips (macOS nativo)
```bash
cd /Users/geminismkt/claude/cityrootsfarm-landing/Images

# Resize imágenes grandes a max 1200px de ancho
for img in 00_*.png Principal_.png Portada.png Taller.png; do
  sips --resampleWidth 1200 "$img"
done

# Hero puede ser más grande (1600px)
sips --resampleWidth 1600 Principal_.png
```

### Paso 2 — Convertir a WebP (si cwebp disponible)
```bash
# Instalar: brew install webp
for img in *.png; do
  cwebp -q 80 "$img" -o "${img%.png}.webp"
done
```

### Paso 3 — HTML con `<picture>` fallback
```html
<picture>
  <source srcset="Images/Principal_.webp" type="image/webp">
  <img src="Images/Principal_.png" alt="..." loading="lazy" width="1200" height="800">
</picture>
```

### Paso 4 — Lazy loading
Agregar `loading="lazy"` a todas las `<img>` excepto:
- Logo en loader (`#pageLoader img`)
- Logo en nav
- Hero image (above the fold)

### Paso 5 — Aspect ratios en CSS
```css
.hero-img { aspect-ratio: 3/4; object-fit: cover; }
.use-img { aspect-ratio: 4/3; object-fit: cover; }
.taller-img { aspect-ratio: 16/9; object-fit: cover; }
```

---

## Tamaños recomendados por contexto

| Contexto | Ancho max | Formato | Calidad |
|----------|-----------|---------|---------|
| Hero background | 1600px | WebP/JPG | 80% |
| Product cards | 800px | WebP/JPG | 80% |
| Recipe cards | 600px | WebP/JPG | 75% |
| Taller card | 800px | WebP/JPG | 80% |
| Flyer modal | 1056px (actual) | PNG | Original |
| Logos/isotipos | Actual | PNG | Lossless |

---

## CSS para imágenes — Patrones actuales

```css
/* Hero */
.hero-img { width: 100%; height: 100%; object-fit: cover; }

/* Recipe cards */
.use-img { width: 100%; height: 100%; object-fit: cover; border-radius: var(--radius-lg) var(--radius-lg) 0 0; }

/* Taller */
.taller-img { width: 100%; height: 280px; object-fit: cover; }

/* Overlay gradient sobre imágenes */
.use-card::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60%;
  background: linear-gradient(to top, rgba(15,43,15,0.85), transparent);
}
```

## Checklist de optimización
- [ ] Resize todas las imágenes > 1 MB
- [ ] Convertir a WebP con fallback PNG/JPG
- [ ] Agregar `loading="lazy"` a imágenes below-the-fold
- [ ] Agregar `width` y `height` a todas las `<img>`
- [ ] Preload hero image
- [ ] Agregar `aspect-ratio` en CSS para prevenir CLS
- [ ] Verificar que nombres coincidan exactamente con el repo (case-sensitive)
