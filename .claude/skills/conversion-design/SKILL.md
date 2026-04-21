# Skill: Diseño para Conversión (CRO)

## Cuándo usar
Para mejorar tasas de conversión, optimizar CTAs, mejorar jerarquía visual orientada a ventas, agregar señales de confianza, o mejorar el flujo de suscripción.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

---

## Embudo actual

```
Visitante → Scroll landing → Click "Ordenar ahora"
  → Step 1: Formulario (datos + CP + variedades)
    → Step 2: Resumen + Botón Mercado Pago
      → Pago en MP → Confirmación
      → (o) WhatsApp ayuda
```

### CTAs principales (puntos de conversión)
| CTA | Ubicación | Acción | Tipo |
|-----|-----------|--------|------|
| "Ordenar ahora" | Nav (desktop + mobile) | `openModal()` | Primario |
| "Empieza tu suscripción" | Hero | `openModal()` | Primario |
| "Suscribirme ahora" | Sección suscripción | `openModal()` | Primario |
| "Continuar al pago →" | Modal Step 1 | `irAlPago()` | Formulario |
| "Suscribirme y pagar" | Modal Step 2 | Link a Mercado Pago | Conversión final |
| "¡Lo quiero!" | Kit de cultivo | WhatsApp | Secundario |
| "Ver flyer del evento" | Talleres | `abrirFlyer()` | Engagement |
| Botón flotante WhatsApp | Esquina inferior der. | wa.me link | Soporte |

---

## Señales de confianza actuales

### Ya implementadas ✅
1. **Datos científicos** — "40× más nutrientes" con fuente USDA/U. Maryland
2. **Precio transparente** — $399/mes visible en hero y sección pricing
3. **Qué incluye** — Lista clara de beneficios de la suscripción
4. **Variedad de pago** — Mercado Pago (tarjeta) + WhatsApp (ayuda personal)
5. **Zona de entrega visible** — CP validation muestra si hay cobertura
6. **Contacto múltiple** — WhatsApp, email, Instagram

### Faltantes (agregar) ❌
1. **Testimonios** — Al menos 3 testimonios de clientes reales con foto/nombre
2. **Número de clientes** — "50+ familias en Monterrey ya reciben sus microgreens"
3. **Garantía** — "Si no te encantan, cancela cuando quieras sin penalización"
4. **Certificaciones/menciones** — Si hay prensa o reconocimientos
5. **FAQ** — Preguntas frecuentes para reducir fricción
6. **Fotos reales de entregas** — Producto llegando a casas reales
7. **Badge de pago seguro** — Logo de Mercado Pago + candado en step 2

---

## Mejoras de conversión recomendadas

### 1. Hero — Above the fold
**Estado actual:** Logo + H1 + 3 stats + botón + imagen
**Mejoras:**
- Agregar **subtítulo más específico** debajo del H1 (propuesta de valor clara)
- Mover un **testimonio corto** al hero ("Lo mejor que he probado" — María, San Pedro)
- Agregar **urgencia suave**: "Cupo limitado por zona" o "Entregas los martes y miércoles"
- CTA con **microcopy** debajo: "Sin compromiso · Cancela cuando quieras"

### 2. Sección Suscripción — Pricing
**Mejoras:**
- Agregar **precio tachado** o comparación: "Menos de $100 por semana"
- **Breakdown del valor**: "$399 ÷ 4 entregas = $99.75 por entrega"
- Agregar **badge "Más popular"** al plan
- Microcopy debajo del CTA: "Pago seguro con Mercado Pago"

### 3. Modal de Suscripción — Reducir fricción
**Step 1 — Formulario:**
- Reducir campos visibles (nombre + teléfono + CP primero, dirección después)
- **Progress indicator** visual: "Paso 1 de 2"
- Preseleccionar variedad más popular en los selects
- Agregar **tooltip** en campo de variedades explicando cada una

**Step 2 — Pago:**
- Agregar **badges de seguridad** junto al botón de MP
- Mostrar **ícono de candado** en el botón
- Agregar texto: "Pago procesado de forma segura por Mercado Pago"
- Deadline suave: "Tu primera entrega puede ser este [día]"

### 4. Sticky CTA en mobile
```css
.sticky-cta-mobile {
  display: none;
}
@media (max-width: 900px) {
  .sticky-cta-mobile {
    display: flex;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 800;
    padding: 1rem 1.5rem;
    padding-bottom: calc(1rem + env(safe-area-inset-bottom));
    background: rgba(250,249,246,0.95);
    backdrop-filter: blur(10px);
    border-top: 1px solid var(--border);
    box-shadow: 0 -4px 20px rgba(0,0,0,0.08);
  }
}
```
- Botón fijo en la parte inferior de la pantalla en mobile
- Aparece después de hacer scroll past el hero
- "Suscríbete — $399/mes" → abre modal

### 5. Exit intent popup (desktop)
- Detectar cuando el mouse sale por arriba de la ventana
- Mostrar popup: "¿Te vas sin probar nuestros microgreens?" + oferta o CTA

### 6. Social proof counter
- Agregar near el CTA principal: "🌱 X personas se suscribieron este mes"
- Puede ser estático al inicio, actualizar manualmente

---

## Jerarquía visual para conversión

### Prioridad de color en CTAs
1. **Verde brillante** (`--green-mid` / `--green-bright`) → Acción principal (suscribirse)
2. **Amber** (`--amber`) → Acción secundaria (talleres, kit)
3. **Outline / transparente** → Acción terciaria (más info, ayuda)
4. **WhatsApp green** (#25d366) → Contacto/soporte

### Tamaño de botones (jerarquía)
- Hero CTA: más grande (1rem font, 1rem 2.5rem padding)
- Sección CTA: mediano (0.92rem font, 0.9rem 2rem padding)
- Card CTA: más pequeño (0.85rem font, 0.7rem 1.5rem padding)

### Whitespace estratégico
- Más espacio alrededor del pricing → dirige la vista al precio
- Menos clutter cerca de CTAs → reduce distracción
- Separación visual entre secciones informativas y secciones de acción

---

## Métricas a monitorear

| Métrica | Herramienta | Cómo |
|---------|-------------|------|
| Clicks en "Ordenar ahora" | Google Analytics / evento | `onclick` event tracking |
| Completados Step 1 | Google Sheets | Registros con CP válido |
| Conversión a MP | Mercado Pago dashboard | Suscripciones activas |
| Ayuda vía WhatsApp | Conteo manual | Mensajes recibidos |
| Scroll depth | Analytics | % que llega a suscripción |
| Bounce rate | Analytics | Abandonos en home |

## Checklist CRO
- [ ] Agregar testimonios (mínimo 3)
- [ ] Microcopy debajo de CTAs ("Sin compromiso", "Pago seguro")
- [ ] Progress indicator en modal (Paso 1/2)
- [ ] Badges de seguridad en step 2
- [ ] Sticky CTA en mobile
- [ ] FAQ section antes del footer
- [ ] Social proof counter cerca del hero
- [ ] Garantía visible ("Cancela cuando quieras")
- [ ] Urgencia suave en sección de suscripción
- [ ] A/B test: color del CTA principal (verde vs amber)
