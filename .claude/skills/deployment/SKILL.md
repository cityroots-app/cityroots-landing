# Skill: Deployment & GitHub Pages

## Cuándo usar
Cuando se necesite desplegar cambios al sitio en producción, resolver problemas de DNS, imágenes rotas en producción, o configurar el repositorio de GitHub Pages.

## Repositorio
- **GitHub:** `https://github.com/cityroots-app/cityroots-landing`
- **Branch:** `main`
- **Dominio:** `cityroots.farm`
- **CNAME file:** contiene `cityroots.farm`
- **DNS:** Configurado en GoDaddy apuntando a GitHub Pages

## Estructura del repo en GitHub
```
cityroots-landing/
├── index.html          (landing page principal)
├── CNAME               (cityroots.farm)
├── Images/
│   ├── 00_Ensalada.png
│   ├── 00_Guacamole.png
│   ├── 00_Hotcakes.png
│   ├── 00_KitdeCultivo.png
│   ├── 00_Produccion.png
│   ├── 00_Sandwich.png
│   ├── 00_Smoothie.png
│   ├── ISOTIPO_VERDE.png
│   ├── ISOTIPO_VERDE_nobg.png
│   ├── LOGO_VERDE.png
│   ├── LOGO_VERDE_nobg.png
│   ├── Portada.png
│   ├── Principal_.png
│   ├── Taller.png
│   └── flyer_taller_25abril.png
```

## Flujo de deployment

### Opción A — Subir por navegador (más fácil)
1. Ir a `https://github.com/cityroots-app/cityroots-landing`
2. Click en archivo → lápiz (✏️) → editar/reemplazar contenido
3. Para imágenes: Add file → Upload files → arrastrar
4. Commit changes
5. Esperar 1-2 minutos, hard refresh (`Cmd+Shift+R`)

### Opción B — Git CLI
```bash
cd /Users/geminismkt/claude/cityroots-landing
git add index.html Images/
git commit -m "Descripción del cambio"
git push origin main
```
**Nota:** Requiere credenciales de GitHub configuradas (token o SSH).

### Clon local del repo
- `/Users/geminismkt/claude/cityroots-landing/` — clon del repo de GitHub
- `/Users/geminismkt/claude/cityrootsfarm-landing/` — carpeta de desarrollo local

Para desplegar, copiar archivos de `cityrootsfarm-landing/` a `cityroots-landing/` y luego push.

## Problemas comunes

### Imágenes no se ven
- **Causa:** Nombre del archivo en el repo no coincide con el `src` en HTML
- **Solución:** Verificar que el nombre exacto (case-sensitive) coincida
- GitHub Pages es case-sensitive: `Images/Taller.png` ≠ `Images/taller.png`
- Verificar que la imagen esté en la carpeta `Images/` del repo (no en la raíz)

### Página muestra contenido incorrecto
- **Causa:** Otro repo de GitHub Pages usando el mismo dominio
- **Solución:** Solo un repo debe tener el CNAME `cityroots.farm`. Eliminar dominio de repos anteriores en Settings → Pages.

### DNS no resuelve
- **Causa:** Registros A/CNAME en GoDaddy mal configurados
- **Solución:** GoDaddy debe tener registros A apuntando a IPs de GitHub Pages:
  ```
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
  ```
  Y CNAME `www` → `cityroots-app.github.io`

### HTTPS no funciona
- **Causa:** Certificado SSL aún no emitido
- **Solución:** Esperar hasta 24 horas después de configurar DNS. Activar "Enforce HTTPS" en repo Settings → Pages.

### Cambios no se reflejan
- **Causa:** Cache del navegador
- **Solución:** Hard refresh `Cmd+Shift+R` o abrir en ventana incógnito

## Archivos que NO subir al repo
- `preview-talleres.html` (solo para desarrollo)
- `flyer_taller_25abril.pdf` (versión PDF, ya hay PNG)
- Archivos `.DS_Store`
- Carpeta `.claude/`
