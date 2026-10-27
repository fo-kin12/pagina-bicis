# Bitácora - Rueda Libre v1.5

## MiniReto 1 - Encabezado y pie con Flexbox
**Qué ocurre al estrechar la ventana**
- Encabezado: en ancho grande, la marca queda a la izquierda y el menú a la derecha (`justify-content: space-between`, `align-items: center`). Al estrechar, los enlaces del menú bajan de línea (`flex-wrap: wrap`) en vez de desbordarse. Bajo 768 px el encabezado pasa a columna: marca arriba y menú centrado debajo.
- Pie: en ancho grande, marca, contacto y horario van en fila, alineados por arriba. Bajo 768 px se apilan en una sola columna.
- El espacio entre enlaces se logra solo con `gap`; ya no hay márgenes ni `inline-block`.

## MiniReto 2 - Flota con Grid
Regla: `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))`, `gap: 1.5rem`, sin media queries.

| Ancho | Columnas |
|---|---|
| 1280 px | 4 |
| 900 px | 3 |
| 500 px | 1 |

> Estos valores salen del cálculo con el contenedor de 1100 px máx. Confírmalos en DevTools (modo dispositivo) y corrige la tabla si ves otra cosa.

- El séptimo modelo (Tándem) se acomodó sin tocar el CSS.
- El enlace "Reservar" queda siempre al final con `margin-top: auto` dentro de la tarjeta flex en columna.

## MiniReto 3 - Flexbox o Grid
| Caso | Herramienta | Por qué |
|---|---|---|
| Portada | Grid (`1fr 1fr`) | Dos columnas, filas y columnas a la vez (dos dimensiones). |
| Galería de rutas | Grid (`repeat(3, 1fr)`) | Tarjetas en cuadrícula que deben tener el mismo ancho. |
| Reservas 2:1 | Grid (`2fr 1fr`) | Proporción exacta entre columnas, definida desde el contenedor. |
| Fila documento + celular | Grid (`1fr 1fr`) | Dos campos del mismo ancho; Grid lo asegura sin depender del contenido. |
| Encabezado | Flexbox | Una sola dimensión (fila); marca y menú en extremos. |
| Pie | Flexbox | Una sola fila de bloques que se reparten el espacio. |

## MiniReto 4 - Problemas responsive
1. **Menú desbordado a 360 px.** Cuatro enlaces con `gap` grande no cabían. Solución: `flex-wrap: wrap` y `justify-content: center`.
2. **Tablas de tarifas con scroll lateral.** Relleno y letra de escritorio. Solución: base móvil con `font-size: .85rem`, relleno de `.35rem` y `overflow-wrap: anywhere`; en 768 px se amplían.
3. **Imágenes que se salían del contenedor.** Solución: `img { max-width: 100%; height: auto; }`, más `width: 100%` y `aspect-ratio` en tarjetas.
4. **Botón de WhatsApp tapaba contenido en móvil.** Solución: tamaño menor (50 px) en base y 60 px desde 768 px.
5. **Fila documento + celular muy estrecha en móvil.** Solución: una columna en base y dos desde 768 px.

Pruebas en DevTools: 360 px, 768 px y 1280 px.
URL de GitHub Pages: _pegar aquí_

## MiniReto 5 - Primera comparación (flota en Bootstrap)
Página: `pruebas/catalogo-bootstrap.html` (Bootstrap 5.3.3 por CDN, sin `styles.css`, sin CSS propio).
Grilla: `row g-4` y por columna `col-12 col-sm-6 col-lg-4 col-xl-3` (1 / 2 / 3 / 4 columnas).

| | CSS propio (v1.5) | Bootstrap |
|---|---|---|
| Tiempo invertido | _anota aquí tu cronómetro_ | _anota aquí tu cronómetro_ |
| Distribución de la flota | 8 declaraciones CSS (`.flota`: 3, `.bici` en columna: 3, `.reservar`: 2) en unas 17 líneas con selectores | 0 líneas de CSS; 13 clases (`row g-4`, `col-12 col-sm-6 col-lg-4 col-xl-3`, `d-flex flex-column gap-2 h-100`, `mt-auto align-self-start`) |

- **Lo que Bootstrap dio sin esfuerzo:** cuatro breakpoints con una sola línea de clases, gutters uniformes, tarjetas de igual altura (`h-100`) y proporción de imagen (`ratio-3x2`).
- **Control perdido:** el ancho mínimo ya no es continuo. Con `auto-fit` mi grilla se ajustaba a cualquier ancho; con Bootstrap solo cambia en los cortes 576, 992 y 1200 px. Entre ellos las tarjetas se estiran o encogen más.
- Las imágenes son URLs absolutas, así que no hubo rutas que ajustar desde `pruebas/`. El enlace "Reservar" usa `../ireservas.html`.

## MiniReto 6 - Utilidades
Sección "¿Por qué Rueda Libre?" y franja "¿Listo para rodar?" sin `style=`, sin `<style>` y sin clases inventadas.
- Franja: columna en móvil y fila desde `md` (`flex-column flex-md-row`), imagen decorativa con `d-none d-md-block`.
- Enlace destacado solo con utilidades: `bg-warning text-dark fw-bold px-4 py-2 rounded-pill text-decoration-none`.

Cinco clases que busqué en la documentación:
1. `object-fit-cover`: recorta la imagen para llenar su caja sin deformarla (nueva en 5.3).
2. `ratio ratio-3x2`: reserva una caja con proporción 3:2 para que todas las fotos midan igual.
3. `text-md-start`: alinea el texto a la izquierda desde `md` (en móvil queda centrado).
4. `d-none d-md-block`: oculta el elemento en móvil y lo muestra desde `md`.
5. `gap-3`: separación uniforme entre hijos de un contenedor flex, en lugar de márgenes.

## MiniReto 7 - Inicio migra a Bootstrap
`index.html` carga `bootstrap.min.css` y después `css/styles.css`. Portada, flota, rutas y beneficios usan `container`, `row`, `g-*` y `col-*`.

Tres conflictos entre mis estilos y los de Bootstrap (y su solución, sin `!important`):
1. **`.btn` chocaba con el componente botón de Bootstrap.** Renombré mi clase a `.btn-marca`.
2. **`* { margin: 0; padding: 0 }` anulaba el reboot de Bootstrap** (márgenes de `p`, `ul`, `h*`). Lo eliminé; el espaciado ahora sale de `mb-*`, `p-*` y `gap-*`.
3. **`.lead` y los tamaños de `h1`/`h2` duplicaban reglas de Bootstrap.** Quité mis versiones y uso `.lead`, `display-5` y `h5`. Dejé en mi hoja solo el color de los títulos.

> Confirma estos tres en DevTools (pestaña Styles, declaraciones tachadas) y ajusta el texto si viste otros.

## MiniReto 8 - Migración completa y comparativa
- **Tarifas:** las dos tablas van en `col-12 col-lg-6`. Conservé mis estilos de tabla (`.tabla-tarifas`) y añadí `caption-side: top`, porque el reboot de Bootstrap pone el título abajo.
- **Reservas:** formulario `col-12 col-md-8`, contacto `col-12 col-md-4`; documento y celular en `row g-3` con `col-12 col-sm-6`. Mantuve `required`, `minlength`, `pattern`, `type="date"` y `min`, y quité `novalidate`, así que valida el navegador.
- **Pie y WhatsApp:** pie en `row` con `col-md-4`; el botón de WhatsApp se posiciona con `position-fixed bottom-0 end-0 m-3`.

| Medida | v1.5 | v2.0-beta |
|---|---|---|
| Líneas de `styles.css` | 352 | 93 |
| Reducción | | 74 % |
| Tiempo invertido | _anota aquí_ | _anota aquí_ |

**Ventajas:** casi cuatro veces menos CSS propio, breakpoints y gutters listos, y distribución consistente en las tres páginas.
**Desventajas:** HTML más largo (muchas clases por elemento), carga de unos 230 KB de CSS y JS de Bootstrap, choques de nombres (`.btn`, `.lead`) y cortes de breakpoint menos flexibles que `auto-fit`.

**Conclusión:** Bootstrap redujo el CSS propio y el trabajo de distribución, y lo que quedó en `styles.css` es solo la identidad visual. El costo es HTML más cargado y depender de los breakpoints de la librería. _Completa con tus tiempos reales._

URL de GitHub Pages v2.0-beta: _pegar aquí_

## MiniReto 9 - Navbar y botones
- Las tres páginas usan `navbar navbar-expand-lg` con `navbar-toggler` y `collapse navbar-collapse`. Por debajo de 992 px aparece el botón de menú; desde 992 px los enlaces se ven directos.
- `data-bs-target="#menuPrincipal"` coincide con `id="menuPrincipal"` en las tres páginas. El toggler tiene `aria-controls`, `aria-expanded` y `aria-label`.
- Página actual: `class="nav-link active"` y `aria-current="page"` (reemplaza la clase `active` de la semana 1).
- El bundle de JavaScript está una sola vez por página, antes de `</body>` (verificado con `grep`: 1 por archivo).
- **Criterio de botones:** principal (`btn-primary`) para acciones de reservar; contorno (`btn-outline-primary`) para consultar ("Ver tarifas", "Ver recorridos", "Ver políticas"); `btn-lg` para la acción principal de cada bloque y `btn-sm` dentro de las cards y en el botón de políticas; `btn-naranja` (propio) solo en la franja final.
- Prueba de teclado a 360 px: _anota aquí el resultado (Tab hasta el botón, Enter abre, Tab recorre enlaces, Enter en el botón cierra)._

## MiniReto 10 - Cards y carrusel
- Flota y rutas ahora son `card h-100` dentro de la grilla; el cuerpo es `card-body d-flex flex-column` y el botón lleva `mt-auto`, así que quedan alineados al final.
- Carrusel `data-bs-ride="carousel"` con las tres promociones, cada imagen con `alt`, y controles con `visually-hidden` ("Promoción anterior" / "Promoción siguiente").
- En `styles.css` eliminé `.bici`, `.precio`, `.reservar`, `.ruta` y `.nivel` (ya no se usan). Ahora los precios usan `text-primary fw-bold` y los niveles `badge text-bg-primary`.

## MiniReto 11 - Formulario, alerta y modal
- Controles con `form-label`, `form-control`, `form-select`, `form-check*`; formulario con `needs-validation novalidate`. Conservé `required`, `minlength`, `pattern`, `type="date"`, `min` y agregué `max`.
- El script de validación es el de la documentación de Bootstrap, copiado sin cambios; otro script aparte pone los límites de fecha y confirma el envío cuando todo es válido.
- Mensajes de error por campo (ej. documento: "solo números, entre 6 y 12 dígitos, sin puntos ni espacios").
- Alerta `alert-info alert-dismissible` con `btn-close` y `data-bs-dismiss="alert"`.
- Modal "Políticas de alquiler" con las cinco políticas en lista, abierto desde un botón junto a la casilla de aceptación; cierra con su botón, con "Entendido" y con Esc.
- Prueba (formulario vacío, corregir campos, modal con mouse y teclado): _anota aquí el resultado._

## MiniReto 12 - Identidad propia y v2.0
**Variables de marca** (`:root`): `--bs-primary` y `--bs-primary-rgb` (Verde Ruta `#1E7A4F`), `--bs-body-font-family` (Open Sans), `--bs-link-color` (y su RGB y hover), `--bs-border-radius`, además de colores de cuerpo y fondo (Azul Noche sobre Crema).
**Botones:** `.btn-primary` y `.btn-outline-primary` reciben Verde Ruta mediante `--bs-btn-*` con hover y active más oscuros; `.btn-naranja` (Naranja Pedal `#F39A2B` con texto Azul Noche) es la variante propia.
**Clases propias:** `.portada-marca`, `.titulo-seccion`, `.pie-marca`, `.ratio-promo`, `.btn-naranja`. No modifican clases de Bootstrap.
**Bootstrap Icons 1.11.3** por CDN: navbar (`bi-bicycle`), contacto (`bi-whatsapp`, `bi-envelope`, `bi-geo-alt`, `bi-clock`), beneficios (`bi-shield-check`, `bi-lock`, `bi-signpost-split`, `bi-geo-alt`) y botón de WhatsApp. Todos llevan `aria-hidden="true"`; el enlace de WhatsApp, que solo tiene ícono, tiene `aria-label`.

**Contraste (fórmula WCAG, calculado a mano; confírmalo con una herramienta):**

| Texto / fondo | Relación |
|---|---|
| Blanco / Verde Ruta (botón principal, franja, badges) | ≈ 5,3:1 |
| Verde Ruta / Crema (enlaces, botón de contorno) | ≈ 4,9:1 |
| Azul Noche / Naranja Pedal (`.btn-naranja`, enlace activo del menú) | ≈ 7,2:1 |
| Azul Noche / Crema (texto del cuerpo) | ≈ 14:1 |
| Blanco / `#0F7B6C` (botón de WhatsApp) | ≈ 5,2:1 |

El verde oficial de WhatsApp (`#25D366`) con ícono blanco daba solo 2:1, por eso uso `#0F7B6C`. Además cambié `text-secondary` por `text-body-secondary` porque el gris por defecto sobre Crema quedaba en ≈ 4,3:1.

**Lista de errores comunes (verificada con `grep`):**
- `!important` en `styles.css`: 0.
- Clases de Bootstrap 4 (`ml-*`, `mr-*`, `float-left`, `data-toggle`...): 0.
- `styles.css` después de Bootstrap en las tres páginas: sí.
- Bundle una sola vez por página: sí.
- `style=` o `<style>` en las páginas: 0.
- Grillas con `container > row > col`: sí en todas.
- Archivo de Bootstrap modificado: no (solo CDN).

**Pruebas finales:** tres anchos (360, 768, 1280), teclado y contraste: _anota aquí lo que observaste._
**Capturas:** `img/capturas/` (instrucciones en `LEEME.md`). _Pendiente: tomar "antes" y "después"._
URL de GitHub Pages v2.0: _pegar aquí_

## Cierre de la semana
- **Retos 1 a 4:** el sitio pasó a un layout responsive con Flexbox, Grid y mobile-first en CSS propio (v1.5, 352 líneas).
- **Retos 5 a 8:** la misma base migró a la grilla y utilidades de Bootstrap; el CSS propio bajó a 93 líneas y se documentaron los conflictos de cascada.
- **Retos 9 a 12:** navbar colapsable, botones unificados, cards, carrusel, validación visual, alerta, modal e identidad de marca con variables, clases propias e íconos.
- **Aprendizajes:** conviene sobrescribir las variables `--bs-*` en vez de reescribir clases del framework; el contraste hay que revisarlo con los colores de marca (el verde de WhatsApp y el gris secundario no lo cumplían); y la validación visual de Bootstrap se apoya en las reglas nativas del HTML.
- **Siguiente semana:** landing "Rueda Nocturna" con Tailwind CSS.

## Commits sugeridos
```bash
# v1.5 (retos 1 a 4)
git init && git add css/styles.css && git commit -m "MiniReto 1: encabezado y pie con Flexbox"
git add index.html && git commit -m "MiniReto 2: flota en Grid con 7 modelos"
git add ireservas.html tarifas.html && git commit -m "MiniReto 3: layout con Grid y Flexbox"
git add . && git commit -m "MiniReto 4: v1.5 mobile-first" && git push

# v2.0-beta (retos 5 a 8)
git add pruebas/ && git commit -m "MiniReto 5: flota con grilla de Bootstrap"
git commit -am "MiniReto 6: beneficios y franja con utilidades"
git add index.html css/styles.css && git commit -m "MiniReto 7: inicio migrado a Bootstrap"
git add . && git commit -m "MiniReto 8: v2.0-beta migración completa" && git push

# v2.0 (retos 9 a 12): haz un commit después de cada reto con los cambios de ese reto
git commit -am "MiniReto 9: navbar colapsable y botones"
git commit -am "MiniReto 10: cards y carrusel de promociones"
git commit -am "MiniReto 11: validación visual, alerta y modal"
git add . && git commit -m "v2.0: identidad de marca, íconos y lanzamiento" && git push
```
