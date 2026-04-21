# Skill: Delivery Zones & Postal Codes

## Cuándo usar
Cuando se necesite agregar, quitar o modificar códigos postales de entrega, cambiar días de entrega, o modificar la lógica de validación de CP.

## Archivo
`/Users/geminismkt/claude/cityrootsfarm-landing/index.html`

## Estructura de datos (~línea 1200)

### ZONAS_ENTREGA
Objeto con CPs como llaves. Cada valor tiene `zona` (nombre legible) y `grupo` (`sp` o `sur`).
```javascript
const ZONAS_ENTREGA = {
  '66220': { zona: 'Del Valle · San Pedro', grupo: 'sp' },
  // ...
  '64920': { zona: 'Del Paseo Residencial', grupo: 'sur' },
  // ...
};
```

### DIAS_ENTREGA
```javascript
const DIAS_ENTREGA = { sp: 'Miércoles', sur: 'Martes' };
```

## Grupos actuales

### San Pedro Garza García (grupo `sp`) → Entrega: Miércoles
| CP | Zona |
|----|------|
| 66220 | Del Valle · San Pedro |
| 66225 | Del Valle / La Joya |
| 66250 | Bosques del Valle / Jerónimo Siller |
| 66254 | Carrizalejo |
| 66256 | Lomas del Valle |
| 66257 | Del Valle sector Fátima |
| 66259 | Gómez Morín / Carrizalejo |
| 66260 | San Agustín / Valle Oriente |
| 66263 | Zona Campestre / Santa Engracia |
| 66265 | Valle del Campestre |
| 66266 | Santa Bárbara / Valle Oriente norte |
| 66267 | Santa Engracia |
| 66268 | Del Valle sector norte |
| 66270 | Colinas de San Agustín / San Patricio |
| 66273 | Montebello / Corp. Santa Engracia |
| 66274 | Jardines de San Agustín |
| 66275 | Los Encinos de San Agustín |
| 66278 | Valle Oriente / Real del Valle / Privanzas |
| 66279 | Punto Central |
| 66280 | Balcones del Valle / Colinas Sierra Madre |
| 66285 | Valle San Ángel / Villas del Valle |
| 66286 | Sierra del Valle / El Santuario |
| 66287 | Lomas del Rosario |
| 66290 | Privada Bosques / zona alta sur |

### Sur de Monterrey (grupo `sur`) → Entrega: Martes
| CP | Zona |
|----|------|
| 64700 | Tecnológico / Roma / zona Tec |
| 64750 | Corredor sur-centro |
| 64830 | Nuevo Sur / Ladrillera |
| 64840 | Altavista / Garza Sada |
| 64845 | Contry Lux |
| 64858 | Contry los Naranjos |
| 64860 | Contry / Los Estanques |
| 64865 | Contry los Nogales |
| 64890 | Satélite / Contry sur / Las Torres |
| 64920 | Del Paseo Residencial |
| 64980 | Carretera Nacional / Esfera / Pueblo Serena |
| 64985 | La Rioja · Residenciales |
| 64986 | Lagos del Vergel |
| 64988 | Carretera Nacional sur |
| 64989 | Sierra Alta / Carretera Nacional |
| 64990 | La Estanzuela / El Uro |

## Cómo agregar un nuevo CP

1. Buscar el objeto `ZONAS_ENTREGA` en el JS (~línea 1200)
2. Agregar entrada: `'XXXXX': { zona: 'Nombre de la zona', grupo: 'sp' | 'sur' },`
3. Respetar orden numérico dentro de cada grupo
4. El grupo determina el día: `sp` = Miércoles, `sur` = Martes

## Cómo agregar un nuevo grupo/día

1. Agregar nuevo grupo en `DIAS_ENTREGA`: `{ sp: 'Miércoles', sur: 'Martes', nuevo: 'Jueves' }`
2. Asignar CPs con `grupo: 'nuevo'`
3. Actualizar el texto informativo en el div `#dia-info` (HTML, ~línea 1140)
4. Actualizar mensaje de error en validación (~línea 1285)

## Validación en tiempo real (~línea 1250)
- Se ejecuta en el evento `input` del campo `#f-cp`
- Solo permite números (strips non-digits)
- Requiere 5 dígitos para validar
- Si CP existe en ZONAS_ENTREGA:
  - Muestra zona + día asignado
  - Llena `#f-dia` automáticamente
  - Pone borde verde + checkmark
- Si no existe:
  - Muestra mensaje "Lo sentimos, aún no entregamos en tu zona"
  - Limpia `#f-dia`
  - Pone borde rojo + X
