# Registro de cambios — reorganización de 2026-07-30

Reescritura completa del proyecto, por fases. Ningún comando de Git fue
ejecutado: todos los cambios son locales y quedan pendientes de commit.

Estado de partida documentado en [`auditoria.md`](auditoria.md).

---

## Fase 1 — Auditoría

- Inventariados 1 HTML, 1 CSS, 0 JS, 2 imágenes y 13 archivos de fuentes.
- Detectados 12 defectos de HTML, 15 de CSS y 5 bloques de Lorem ipsum.
- Escrito `docs/auditoria.md` con el inventario y las desviaciones del README
  respecto al código.

## Fase 2 — Estructura

| Antes | Después |
|---|---|
| `estilos.css` | `assets/css/base.css` + `layout.css` + `components.css` |
| `IMG/Corona.png` | `assets/img/logo/corona-emblem.webp`, `favicon.png`, `apple-touch-icon.png` |
| `IMG/20.jpg` | `assets/img/content/chrome-abstract.webp` |
| `Fonts/Cinzel-VariableFont_wght.ttf` | `assets/fonts/cinzel-variable.woff2` |
| `Fonts/OFL.txt` | `assets/fonts/ofl.txt` |
| — | `404.html`, `robots.txt`, `sitemap.xml`, `.gitignore` |

- Eliminadas las carpetas `IMG/` y `Fonts/`.
- Todos los nombres pasados a minúsculas con guiones; ningún archivo conserva
  mayúsculas, espacios ni números de versión.
- Todas las rutas de HTML y CSS actualizadas y verificadas contra disco.
- No se creó `assets/js/`: el proyecto no tiene JavaScript y la estructura
  admite adaptarse cuando el proyecto es más simple. No se crean carpetas
  vacías.
- No se creó `assets/css/pages/`: ninguna página necesita CSS propio.

## Fase 3 — Higiene

- Eliminados 10 archivos de fuentes huérfanos (835 KB que ninguna regla CSS
  cargaba jamás):
  - 4 de Dancing Script. Además se redistribuían **sin su licencia OFL**: el
    único `OFL.txt` del repo es el de Cinzel.
  - 6 estáticos de Cinzel, redundantes frente al archivo variable.
- Eliminado `Fonts/README.txt`: boilerplate de Google Fonts que describía una
  carpeta `static/` inexistente en el repo.
- Eliminados los dos originales de imagen tras convertirlos; ninguno queda
  declarado como fallback, así que no se conservan.
- Creado `.gitignore` para stack estático: SO, editores, logs, `node_modules/`
  (que `npx serve` puede generar), `.env`, `.vercel/` y `dist/`.
- Búsqueda de credenciales, tokens y claves: **ninguna encontrada**. El
  proyecto no tiene backend ni llamadas de red.
- Formato normalizado: indentación de 2 espacios, comillas dobles en HTML,
  salto de línea final en los 8 archivos de texto.

## Fase 4 — Imágenes

| Archivo | Antes | Después | Reducción |
|---|---|---|---|
| Obra abstracta | `20.jpg`, 1080×1351, 1 010 KB | `chrome-abstract.webp`, 800×1001, 171 KB | −83 % |
| Emblema (en página) | `Corona.png`, 1024×1024, 390 KB | `corona-emblem.webp`, 256×256, 9,7 KB | −97,5 % |
| Favicon | el mismo PNG de 390 KB | `favicon.png`, 96×96, 7,8 KB | −98 % |
| Touch icon | no existía | `apple-touch-icon.png`, 180×180, 19,6 KB | — |

- **1 400 KB → 189 KB** en la carga inicial (favicon incluido).
- `width` y `height` declarados en los dos `<img>`, con los valores intrínsecos
  reales del archivo, para eliminar el layout shift.
- Ningún `loading="lazy"`: las dos imágenes están en la primera pantalla. La
  obra lleva `fetchpriority="high"` por ser el elemento LCP.
- `alt` reescritos: la obra describe lo que se ve; el emblema pasa a `alt=""`
  porque la palabra «Corona» ya está en texto justo al lado.
- Nombres semánticos. `.BeerIMG` describía como cerveza una pieza abstracta.
- Añadida `assets/img/content/og-card.png` (1200×630, 31 KB), compuesta a
  partir del emblema y de los dos colores que ya usaba el sitio. No contiene
  ningún elemento ajeno al proyecto.

## Fase 5 — HTML, SEO y accesibilidad

- Eliminado el `<input type="color">` que estaba **entre `</head>` y `<body>`**,
  resto de depuración que se publicaba como un selector de color visible.
- Corregido el `viewport`, que tenía las claves sin comas y usaba
  `max-scale`/`min-scale`, que no existen. Eliminado `user-scalable=no`, que
  bloqueaba el zoom táctil e incumplía WCAG 2.1 SC 1.4.4.
- Titular reconstruido: de dos `<h2>` con fragmentos de palabra más una tercera
  copia oculta por `visibility`, a **un solo `<h1>`** con dos `<span>`. El
  nombre accesible se restaura con `aria-label` sin duplicar texto en el DOM.
  Verificado en el árbol de accesibilidad de Chrome: `heading nivel 1 = "Living
  is a sensory experience"`.
- Landmarks correctos y en orden de lectura: `header` → `main` → `footer`.
  Antes el `<header>` estaba escrito después del `<main>`.
- `<head>` completo: `title` de 55 caracteres, `description` de 158, canonical,
  Open Graph (con `og:image` real), `twitter:card`, `theme-color`, favicon y
  apple-touch-icon.
- Eliminado el nav: sus 3 ítems eran `<li>` sin `<a>`, sin destino y sin acceso
  por teclado, y no existe ninguna página a la que pudieran llevar.
- Eliminado el falso botón «SUSCRIBE»: era un `<p>` con `cursor: pointer`,
  sombra y animación, no conectado a ningún servicio.
- Las tres barras decorativas conservan su papel gráfico pero pierden el
  `cursor: pointer` y el hover que las hacían parecer un menú. Van con
  `aria-hidden="true"`.
- Eliminados los 5 bloques de Lorem ipsum, incluido el `div.intentodecuadro`,
  que no tenía ninguna regla CSS y se imprimía como texto negro sobre el
  diseño.
- Creado `404.html` con el mismo sistema visual, el mismo recurso tipográfico
  aplicado a «not found» y un botón real de vuelta a `index.html`.
- Creados `robots.txt` y `sitemap.xml` con la URL real del sitio.

## Fase 6 — CSS y sistema de diseño

- Un único archivo de 438 líneas sustituido por tres de 562 en total, cada uno
  ordenado como variables → reset → base → layout → componentes → media
  queries.
- Extraídos a `:root`: 4 colores, 2 familias, 6 tamaños de texto, 3 tracking,
  2 interlineados, la escala de espaciado, el gutter, la medida de línea, el
  radio, el grosor de borde y la transición.
- Paleta derivada de los colores que el sitio ya usaba: `#ffc31f` y `#09003b`.
  No se inventó ninguno. Contraste del par: **12,2:1**.
- Escala de espaciado 4 / 8 / 16 / 24 / 32 / 48 / 64 / 96. Eliminados los
  valores mágicos: `top: -2%`, `left: -8.6%`, `bottom: 18.7%`, `left: 24.9%`,
  `right: 42.7%`, `width: 44.2%`, `height: 42.5%`.
- Dos familias como máximo: Cinzel para el display y la pila del sistema para
  el resto. Antes, Dancing Script (caligráfica) se aplicaba a `html`, es decir
  también a párrafos de 12 px.
- `@font-face` movido antes de su uso, con `font-display: swap`.
- Eliminados: el paso `0% { }` vacío del `@keyframes`, las declaraciones
  repetidas de `.Color`/`.Color2` en media query y los `padding: 0; margin: 0;`
  repetidos en 9 selectores.
- Ningún selector supera los 3 niveles. Ningún estilo inline. El único
  `!important` del proyecto está en el bloque `prefers-reduced-motion`, donde
  es el patrón estándar.
- Nomenclatura BEM coherente en lugar de `intentodecuadro`, `thisisaSEC`,
  `UnAside`, `Div0/1/2`, `HeaderInformationVIS` o `SUBscription`.

## Fase 7 — Responsive

- Invertido a mobile-first: las 12 media queries `max-width` con 7 breakpoints
  arbitrarios (376, 395, 450, 496, 603, 800, 1137) pasan a 3 `min-width` en
  480 / 768 / 1024.
- Sustituido el posicionamiento absoluto por CSS Grid. La división de color ya
  no está fijada al 50 %: la dibujan los propios elementos, así que cae
  exactamente en el corte de la palabra a cualquier ancho.
- **Corregido un desbordamiento vertical**: dos filas `1fr` en un contenedor de
  altura indefinida se igualan entre sí, así que la fila vacía superior tomaba
  los mismos 281 px que la de la obra. Abría una franja dorada muerta y estiraba
  el documento a 943 px en un viewport de 640. Resuelto dejando una sola pista
  flexible.
- Verificado sin scroll horizontal en 360, 480, 768, 1024 y 1440 px con
  `document.documentElement.scrollWidth > window.innerWidth`, y sin scroll
  vertical: el documento mide exactamente el alto del viewport en las 10
  combinaciones.
- Áreas táctiles: 44 px en el botón; 32 px en los enlaces de texto en línea,
  ampliadas con padding vertical que no altera el interlineado. Los enlaces
  dentro de un párrafo están exentos del mínimo de WCAG 2.2 SC 2.5.8.

## Fase 8 — UX / UI

- Jerarquía en una pantalla: marca, titular, hashtag, obra y créditos. Sin
  scroll.
- El único CTA con destino real es el botón de `404.html`, que vuelve al
  inicio. En la landing no se inventó ninguno: no hay producto, tienda ni
  formulario a los que enlazar.
- Estados completos en todo lo interactivo: default, hover (solo bajo
  `@media (hover: hover)`, para que no se queden pegados en táctil), focus
  visible con anillo de 2 px y 3 px de separación, y active. Transiciones de
  180 ms.
- Sin gradientes decorativos: el único `linear-gradient` es la división de
  color con paradas duras, que es el diseño. Eliminadas la sombra de 10 px y la
  animación de latido infinita del antiguo botón.
- Ancho de línea limitado a 68 caracteres.
- No hay formularios. El anterior «SUSCRIBE» no estaba conectado a nada y se
  eliminó en lugar de simularlo.

## Fase 9 — JavaScript

- El proyecto sigue sin JavaScript. No se añadió ninguno: sin nav que plegar y
  sin formularios, no había nada que justificara un archivo.
- Cero errores y cero avisos de consola en las 10 combinaciones de página y
  viewport servidas por HTTP.

## Fase 10 — Rendimiento

| Métrica | Antes | Después |
|---|---|---|
| Imágenes | 1 400 KB | 189 KB |
| Fuentes | 916 KB en disco, 81 KB cargados | 22,7 KB |
| Primera carga | 1 491 KB | **227 KB** (−85 %) |
| Peticiones | 5 | 8 |

- Cinzel variable convertida de TTF a WOFF2 y subseteada a latín con
  `fonttools`: 124,7 KB → 22,7 KB conservando el eje `wght` 400–900 completo.
- `preload` de la fuente con `crossorigin`, y `font-display: swap`. No hace
  falta `preconnect`: la fuente es del mismo origen.
- CSS repartido en tres archivos según la estructura estándar. Suman 12,3 KB;
  el coste de las dos peticiones extra es menor que la pérdida de legibilidad
  de un único archivo.
- Ninguna librería ni CDN. El proyecto ya era autocontenido y lo sigue siendo.

## Fase 11 — QA

Verificado con Chrome sin cabeza sobre servidor local, en `/index.html` y
`/404.html`, a 360×640, 480×800, 768×1024, 1024×768 y 1440×900:

- Todos los enlaces del pie resuelven; los internos apuntan a archivos que
  existen en disco.
- Las 8 rutas de recursos devuelven 200. Cero peticiones fallidas.
- Cero mensajes de consola, cero excepciones, cero imágenes rotas.
- Sin scroll horizontal ni vertical en ninguna combinación.
- Un solo `h1` por página, con nombre accesible correcto.
- `title` y `description` únicos por página, de 55/51 y 158/158 caracteres.
- Recorrido completo con Tab: los 3 enlaces de `index.html` y los 4 de
  `404.html` reciben `:focus-visible` con anillo contrastado.
- Sin Lorem ipsum, sin restos de plantilla, sin credenciales.

## Fase 12 — Documentación

- `README.md` reescrito. El anterior describía un proyecto distinto del código:
  afirmaba que Cinzel era la tipografía del titular y del nav cuando ninguna
  regla la declaraba, contaba cuatro ítems de nav donde había tres, y
  presentaba el `<input type="color">` olvidado como una decisión de diseño.
  Ahora todas esas afirmaciones son ciertas o han desaparecido.
- Eliminadas las insignias de shields.io: eran decorativas y no aportaban nada
  que el texto no diga.
- Creado este `docs/cambios.md`.

## Fase 13 — Deploy

- Sin rutas absolutas de máquina en ningún archivo.
- Las 13 rutas internas son relativas y están en minúsculas.
- Verificado servido por HTTP y abierto directamente desde disco. En `file://`
  todo se renderiza salvo la fuente: Chrome trata los documentos `file://` como
  origen opaco y bloquea la petición de `@font-face`, así que el titular cae al
  serif de reserva. Es una restricción del navegador, no del proyecto.
- No se creó configuración de hosting (`vercel.json`, `_redirects`,
  `.htaccess`): no se indicó destino.
- **No se ejecutó ningún comando de Git ni se desplegó nada.**
