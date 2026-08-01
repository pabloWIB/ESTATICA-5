# Auditoría — Corona Lifestyle Landing

Fecha: 2026-07-30
Estado analizado: commit `74cb447` (`Feat favicon`), con `README.md` modificado sin confirmar.
Documento de trabajo interno. Refleja el estado **previo** a la reorganización.

---

## 1. Inventario de archivos

### 1.1 HTML

| Archivo | `<title>` | `<h1>` | Propósito real | Estado |
|---|---|---|---|---|
| `index.html` | `Corona` | `Corona` | Landing de una sola pantalla para una marca ficticia de bebida. Composición tipográfica partida en dos mitades de color. | Único archivo HTML. 66 líneas. Estructura inválida y contenido de relleno. |

No existe `404.html`. No existe ninguna otra página.

### 1.2 CSS

| Archivo | Peso | Se carga | Reglas | Observaciones |
|---|---|---|---|---|
| `estilos.css` | 6,4 KB | Sí, desde `index.html:7` | 38 bloques | Todo el proyecto en un único archivo. Posicionamiento absoluto al 100 %. 12 media queries `max-width`. |

No hay CSS huérfano: solo existe un archivo y sí se carga.

### 1.3 JavaScript

| Archivo | Estado |
|---|---|
| — | **No existe ningún archivo JS en el proyecto.** No hay `<script>` en el HTML. |

### 1.4 Imágenes

| Ruta | Peso | Dimensiones | Formato | Uso real | Problema |
|---|---|---|---|---|---|
| `IMG/Corona.png` | 399,9 KB | 1024 × 1024 | PNG (con alfa) | Solo como favicon (`index.html:8`) | 400 KB para un icono de 32 px. Declarado con MIME incorrecto (`image/x-icon` en un PNG). |
| `IMG/20.jpg` | 1 034,4 KB | 1080 × 1351 | JPEG | Visual principal, `index.html:59` | **1 MB en una sola imagen.** Se renderiza a ~30 % del ancho de viewport (≈380 px en 1280 px) — se sirven 1080 px para mostrar 380. Sin `width`/`height`, sin `loading`, `alt=""`. Nombre no semántico. Lleva marca de agua de terceros: *"by @songsandthespirits"*. |

**Peso total de imágenes: 1,40 MB.** El objetivo del proyecto es < 1 MB de primera carga; solo las imágenes ya lo superan.

Ninguna imagen está rota: ambas rutas existen en disco.

### 1.5 Fuentes

| Archivo | Peso | Referenciado en CSS | Estado |
|---|---|---|---|
| `Fonts/DancingScript-Regular.ttf` | 81,2 KB | Sí (`estilos.css:11`) | En uso — es la única fuente aplicada en todo el sitio. |
| `Fonts/DancingScript-Medium.ttf` | 81,2 KB | No | **Huérfano** |
| `Fonts/DancingScript-SemiBold.ttf` | 81,2 KB | No | **Huérfano** |
| `Fonts/DancingScript-Bold.ttf` | 81,2 KB | No | **Huérfano** |
| `Fonts/Cinzel-Regular.ttf` | 76,5 KB | No | **Huérfano** |
| `Fonts/Cinzel-Medium.ttf` | 76,7 KB | No | **Huérfano** |
| `Fonts/Cinzel-SemiBold.ttf` | 76,8 KB | No | **Huérfano** |
| `Fonts/Cinzel-Bold.ttf` | 76,8 KB | No | **Huérfano** |
| `Fonts/Cinzel-ExtraBold.ttf` | 76,9 KB | No | **Huérfano** |
| `Fonts/Cinzel-Black.ttf` | 76,9 KB | No | **Huérfano** |
| `Fonts/Cinzel-VariableFont_wght.ttf` | 124,7 KB | No | **Huérfano** |
| `Fonts/OFL.txt` | 4,5 KB | n/a | Licencia — cubre **solo Cinzel** (`Copyright 2020 The Cinzel Project Authors`). |
| `Fonts/README.txt` | 2,2 KB | n/a | Boilerplate de Google Fonts. Referencia una carpeta `static/` que no existe en el repo. |

**916 KB de fuentes, de los cuales 835 KB (91 %) no se usan.** Las 7 variantes de Cinzel están en el repo pero ninguna regla CSS las declara: el `README.md` actual afirma que Cinzel es la tipografía del titular y del nav, y **eso es falso** en el código.

**Problema de licencia:** Dancing Script se redistribuye sin su texto de licencia OFL. El único `OFL.txt` presente es el de Cinzel.

### 1.6 Dependencias externas

Ninguna. Sin CDNs, sin Google Fonts remoto, sin librerías, sin jQuery, sin `node_modules`, sin `package.json`. El proyecto es 100 % autocontenido. Es el punto más fuerte del estado inicial.

### 1.7 Archivos basura

Ninguno. Sin `.bak`, `.DS_Store`, `Thumbs.db`, `copia de`, `final_v2`, `node_modules` ni logs.

---

## 2. Problemas detectados

### 2.1 Enlaces y rutas rotas

| Tipo | Resultado |
|---|---|
| Enlaces `href` a archivos | **Ninguno existe.** El sitio no tiene ni un solo `<a>`. |
| Imágenes `src` rotas | Ninguna. Las 2 rutas resuelven. |
| `<link>` / `<script>` rotos | Ninguno. `estilos.css` y `IMG/Corona.png` existen. |

No hay rutas rotas, pero tampoco hay navegación real: **el sitio no contiene ningún enlace**.

### 2.2 HTML inválido y estructural

| # | Línea | Problema | Gravedad |
|---|---|---|---|
| 1 | `index.html:10` | `<input type="color">` colocado **entre `</head>` y `<body>`**. Elemento de depuración olvidado, en posición inválida. El parser lo reubica dentro de `<body>`, dejando un selector de color visible en la esquina superior izquierda del sitio publicado. | Alta |
| 2 | `index.html:5` | `viewport` mal formado: `initial-scale=1.0 max-scale=1.0 min-scale=1.0` sin comas separadoras, y `max-scale`/`min-scale` no son claves válidas (son `maximum-scale`/`minimum-scale`). | Media |
| 3 | `index.html:5` | `user-scalable=no` bloquea el zoom táctil. Incumple WCAG 2.1 SC 1.4.4 (Resize Text). | Alta |
| 4 | `index.html:34,38` | El titular está partido en **dos `<h2>` con fragmentos de palabra**: `LIVING IS A SEN` y `SORY EXPERIENCE`. Un lector de pantalla anuncia dos encabezados sin sentido. | Alta |
| 5 | `index.html:42` | El mismo titular aparece **una tercera vez** completo en `.HeaderInformationVIS`, oculto/mostrado por media query. Contenido triplicado en el DOM. | Media |
| 6 | `index.html:21-25` | El nav son `<li>` de texto plano, **sin `<a>`**. No son enfocables, no navegan, no responden a teclado. Solo tienen `cursor: pointer`. | Alta |
| 7 | `index.html:53-55` | «SUSCRIBE» es un `<p>` con `cursor: pointer`, sombra y animación de latido. Parece un botón, no es un botón y no hace nada. | Alta |
| 8 | `index.html:27-31` | `.UnAside` (tres barras amarillas decorativas) tiene `cursor: pointer` y `transform: scale()` en hover. Simula un menú hamburguesa que no abre nada. | Media |
| 9 | `index.html:59` | `<img src="IMG/20.jpg" alt="">`. Es el visual principal de la página, no un elemento decorativo, y va con `alt` vacío. Sin `width`/`height` → layout shift. | Media |
| 10 | `index.html:12-14` | `.intentodecuadro` («intento de cuadro») contiene un párrafo de Lorem ipsum y **no tiene ninguna regla CSS**. Se renderiza como texto negro suelto sobre la composición. Resto de pruebas. | Alta |
| 11 | `index.html:1-9` | `<head>` mínimo: sin `description`, sin Open Graph, sin `canonical`, sin `theme-color`. `<title>` de 6 caracteres. | Alta (SEO) |
| 12 | `index.html:32-45` | `<header>` colocado **después** de `<main>` y de `<nav>` en el orden del documento. El orden de lectura no corresponde al orden visual. | Media |

### 2.3 Contenido de relleno

| Ubicación | Contenido |
|---|---|
| `index.html:13` | Lorem ipsum (párrafo completo, ~230 caracteres) |
| `index.html:35` | Lorem ipsum truncado a media palabra: `...adipisicing eillum voluptate` |
| `index.html:39` | Lorem ipsum truncado: `lit. Molestiae quia...` |
| `index.html:43` | Lorem ipsum (párrafo completo) |
| `index.html:49` | Lorem ipsum (párrafo completo) |

**5 de los 6 bloques de texto de la página son Lorem ipsum.** El único texto real del proyecto es: `Corona`, `LIVING IS A SENSORY EXPERIENCE`, `#THISISLIVING`, los 3 rótulos del nav, `SUSCRIBE` y `©Copyright PABLO`.

### 2.4 CSS

| # | Problema | Detalle |
|---|---|---|
| 1 | **Posicionamiento absoluto total** | 14 de 15 bloques visibles usan `position: absolute` o `fixed` con offsets en porcentaje. No hay flujo de documento, ni flex, ni grid. |
| 2 | **Valores mágicos** | `top: -2%`, `left: -8.6%`, `bottom: 18.7%`, `left: 24.9%`, `right: 42.7%`, `width: 44.2%`, `height: 42.5%`. Ajustados a ojo, no derivan de ninguna escala. |
| 3 | **Desktop-first** | Las 12 media queries son `max-width`. Contradice mobile-first. |
| 4 | **Breakpoints arbitrarios** | 800, 1137, 603, 496, 450, 395, 376 px. Siete puntos de corte distintos, ninguno estándar, todos añadidos para parchear un desbordamiento concreto. |
| 5 | **Desbordamiento horizontal** | `.Derechos` (`transform: rotate(270deg)` + `left: -8.6%`) y `.Navigation` en ≤450px (`left: -8.6%`) se salen del viewport. |
| 6 | **Sin variables CSS** | `rgb(255, 195, 31)` se repite 8 veces, `#09003B` 9 veces. Ni un `:root`. |
| 7 | **Reglas duplicadas** | Los bloques `@media (max-width:800px)` de `.Color` y `.Color2` repiten literalmente las 6 declaraciones del bloque base cambiando solo 2 valores. Se repiten `padding: 0; margin: 0;` en 9 selectores. |
| 8 | **`@keyframes` con paso vacío** | `PLZsub` declara un bloque `0% { }` sin ninguna declaración dentro. |
| 9 | **Nomenclatura** | Clases en mezcla de idiomas y capitalizaciones, sin convención: `intentodecuadro`, `thisisaSEC`, `UnAside`, `Div0`/`Div1`/`Div2`, `Color`/`Color2`, `HeaderInformationVIS`, `SUBscription`, `Derechos`, `BeerIMG`. |
| 10 | **Nombre engañoso** | `.BeerIMG` contiene una imagen que no es una cerveza, sino una pieza abstracta de arte digital. |
| 11 | **`visibility: hidden` como layout** | Se muestran/ocultan tres bloques con `visibility` en lugar de tener un solo bloque responsive. El contenido oculto sigue ocupando su caja. |
| 12 | **Contraste** | Los pares de color usados (`#09003B` sobre `#FFC31F` = 12,5:1; `#FFC31F` sobre `#09003B` = 12,5:1; blanco sobre `#09003B` = 17,8:1) **sí cumplen** AA. Es lo único correcto del sistema visual. |
| 13 | **Tipografía en cuerpo de texto** | Dancing Script (caligráfica) aplicada a `html`, es decir a todo el texto incluidos párrafos de 12–16 px. Ilegible a tamaño pequeño. |
| 14 | **`@font-face` tardío** | La declaración `@font-face` está en la línea 9, después de la regla `html { font-family: Dancing }` de la línea 5. Funciona, pero el orden es incorrecto. |
| 15 | **Sin `font-display`** | Sin `font-display: swap` → FOIT mientras carga la fuente. |

### 2.5 HTML duplicado entre páginas

No aplica: solo hay una página. No hay nav ni footer repetidos que extraer.

### 2.6 Archivos de configuración ausentes

| Archivo | Estado |
|---|---|
| `.gitignore` | **No existe** |
| `robots.txt` | **No existe** |
| `sitemap.xml` | **No existe** |
| `404.html` | **No existe** |

### 2.7 Credenciales

Búsqueda de tokens, claves y credenciales en `index.html` y `estilos.css`: **ninguna encontrada**. El proyecto no tiene backend, formularios conectados ni llamadas de red.

### 2.8 Desviaciones del README actual respecto al código

El `README.md` presente describe un proyecto que no coincide con el código:

| Afirmación del README | Realidad en el código |
|---|---|
| «Type is Cinzel… Headline and nav» | Cinzel no está declarada en ningún `@font-face`. La única fuente aplicada es Dancing Script. |
| «Four-item nav: Products, Shop, Merch, About Us» | El nav tiene **tres** ítems: `PRODUCTS`, `SHOP MERCH`, `ABOUT US`. |
| «the full phrase is repeated as a single accessible line» | Se repite, pero como `<h2>` visible/oculto por `visibility`, no como texto accesible. Los fragmentos siguen siendo encabezados reales. |
| «Everything, including the colour input in the markup, is HTML and CSS» | El `<input type="color">` no es una decisión de diseño: está fuera de `<body>`, es un resto de depuración. |
| Estructura: `IMG/ └── 2 assets, including the favicon` | Correcto, pero omite que una de ellas pesa 1 MB. |

---

## 3. Resumen ejecutivo

1. **Qué es**: una landing de marca de una sola pantalla para «Corona», una marca ficticia de bebida. Todo el concepto descansa en un recurso tipográfico: la frase *LIVING IS A SENSORY EXPERIENCE* se parte a mitad de la palabra «SENSORY» justo sobre la línea que divide la pantalla en amarillo y azul marino. Es un ejercicio de composición CSS, no un sitio comercial.
2. **En qué estado está**: prototipo de aprendizaje sin terminar. Cero dependencias externas y contraste de color correcto, pero maquetado íntegramente con `position: absolute` y porcentajes ajustados a ojo, con siete breakpoints improvisados para tapar desbordamientos.
3. **Lo más grave — contenido**: 5 de los 6 bloques de texto son Lorem ipsum, y uno de ellos (`.intentodecuadro`) ni siquiera tiene estilos: se imprime como texto negro suelto encima del diseño. El sitio no es publicable tal cual.
4. **Lo más grave — código**: hay un `<input type="color">` fuera de `<body>`, el titular está partido en dos `<h2>` con fragmentos de palabra, el nav no tiene un solo `<a>` y «SUSCRIBE» finge ser un botón. Nada del sitio es navegable con teclado y `user-scalable=no` bloquea el zoom.
5. **Lo más grave — peso**: 1,40 MB de imágenes (una sola JPEG de 1 MB servida a 380 px de ancho) y 916 KB de fuentes de las que el 91 % no se carga jamás. El README afirma que la tipografía es Cinzel; Cinzel está en el repo pero ninguna regla CSS la declara.
