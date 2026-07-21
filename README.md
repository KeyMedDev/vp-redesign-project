# Vecinos Perú — Sitio Web Institucional

Manual técnico del proyecto de rediseño del sitio web de **Vecinos Perú**, asociación civil sin fines de lucro dedicada al desarrollo rural sostenible en la región andina del Perú desde 1985.

## 1. Descripción general

Sitio web institucional estático (multi-página) que presenta a la organización, sus líneas de acción, proyectos y un formulario de contacto. No requiere backend, framework ni proceso de build: es HTML, CSS y JavaScript servidos directamente.

## 2. Stack tecnológico

| Capa | Tecnología |
|---|---|
| Marcado | HTML5 (semántico: `header`, `main`, `section`, `article`, `footer`) |
| Estilos | CSS3 puro, con variables CSS (`:root`) y `@media queries` |
| Tipografías | Google Fonts (`Poppins`, `Inter`) vía `@import` |
| Interactividad | JavaScript (`src/script/main.js`) — **archivo presente pero vacío** |
| Imágenes | Locales (`src/assets/media/imgs`) y remotas desde Pexels (CDN externo) |
| Build tools | Ninguna. No hay `package.json`, gestor de paquetes ni bundler |

No hay dependencias de terceros que instalar: el proyecto se ejecuta abriendo los archivos HTML directamente o sirviéndolos con cualquier servidor estático.

## 3. Estructura del proyecto

```
vp-redesign-project/
├── index.html                  # Página de inicio
├── src/
│   ├── assets/
│   │   └── media/
│   │       ├── icons/          # (vacío actualmente)
│   │       └── imgs/           # Imágenes locales del sitio
│   ├── pages/
│   │   ├── nosotros.html       # Quiénes somos / Visión / Misión / Fines
│   │   ├── lineas-accion.html  # Líneas de acción institucionales
│   │   ├── proyectos.html      # Proyectos vigentes, cooperantes y concluidos
│   │   └── contactanos.html    # Formulario de contacto
│   ├── script/
│   │   └── main.js             # JS del sitio (sin implementar)
│   └── style/
│       ├── main.css            # Estilos base compartidos (reset, header, nav, hero, etc.)
│       ├── nosotros.css        # Estilos específicos de nosotros.html
│       ├── lineas-accion.css   # Estilos específicos de lineas-accion.html
│       ├── proyectos.css       # Estilos específicos de proyectos.html
│       ├── contactanos.css     # Estilos específicos de contactanos.html
│       └── responsive.css      # Media queries (breakpoint móvil)
└── README.md
```

### Convención de rutas

- `index.html` (raíz) referencia sus recursos con rutas relativas a `src/...` (ej. `src/style/main.css`).
- Las páginas dentro de `src/pages/` referencian recursos con rutas relativas a `../` (ej. `../style/main.css`, `../../index.html`).

Al modificar o mover archivos, respetar esta diferencia de profundidad de rutas para no romper enlaces ni estilos.

## 4. Mapa de navegación

```
Inicio (index.html)
├── Nosotros (nosotros.html)
│   ├── #quienes-somos
│   ├── #vision
│   ├── #mision
│   └── #fines-objetivos
├── Líneas de Acción (lineas-accion.html)
├── Proyectos (proyectos.html)
│   ├── #vigentes
│   ├── #cooperantes
│   └── #concluidos
└── Contáctanos (contactanos.html)
```

El menú de navegación (`<header><nav>`) se repite de forma manual e idéntica en **cada** archivo HTML (no hay includes ni componentización). Cualquier cambio en el menú debe replicarse en los 5 archivos HTML.

## 5. Sistema de diseño (CSS)

Definido mediante variables CSS en `src/style/main.css`:

```css
:root {
    --color-principal: #4f7a3b;       /* Verde institucional */
    --color-principal-oscuro: #375528;
    --color-acento: #e8b74d;          /* Dorado/acento */
    --color-texto: #1f2a1a;
    --color-texto-suave: #5c6b57;
    --color-fondo: #ffffff;
    --color-fondo-suave: #f4f7f1;
    --color-borde: #e3e8de;
    --espaciado: 24px;
    --radio: 14px;
}
```

- **Tipografías:** `Poppins` (títulos `h1`–`h3`) e `Inter` (cuerpo de texto), cargadas desde Google Fonts.
- **`main.css`** contiene el reset básico, el header/nav, el hero de inicio y clases reutilizables entre páginas (`.dos-columnas`, `.imagen-seccion`, `.boton`, `.etiqueta`, `.grid-tarjetas`, `.tarjeta`, etc.).
- Cada página tiene además su propio archivo CSS para estilos exclusivos de esa vista.
- **`responsive.css`** define un único breakpoint (`max-width: 700px`) que ajusta tamaños de fuente, espaciado del nav y apila `.dos-columnas` en vertical. Se carga después de `main.css` en el `<head>` de cada página (verificar orden de `<link>` para que las reglas responsivas tengan prioridad de cascada).

## 6. Componentes reutilizables (clases CSS clave)

| Clase | Uso |
|---|---|
| `.hero`, `.hero-overlay`, `.hero-contenido` | Sección de portada con imagen de fondo y texto superpuesto |
| `.dos-columnas` (+ `.invertida`) | Layout de dos columnas texto/imagen, usado en `nosotros.html` |
| `.grid-tarjetas` / `.tarjeta` | Grid de tarjetas para las líneas de acción |
| `.proyecto` / `.proyecto-final` | Bloques de listado de proyectos |
| `.boton` | Botón de llamada a la acción (CTA) |
| `.etiqueta` / `.etiqueta-clara` | Badges de texto destacado |
| `.li-submenu` / `.submenu` | Submenú desplegable en la navegación |
| `.encabezado-pagina` | Cabecera introductoria de las páginas internas |

## 7. Estado actual / pendientes conocidos

- `src/script/main.js` **está vacío**. No hay lógica implementada para: envío del formulario de contacto, apertura/cierre de submenús en móvil, ni interactividad adicional.
- El formulario de `contactanos.html` no tiene `action` ni `method`, ni manejador JS: actualmente **no envía datos a ningún destino**.
- La carpeta `src/assets/media/icons/` existe pero está vacía.
- Varias imágenes (`lineas-accion`, `proyectos`, secciones de `nosotros.html`) se cargan desde **Pexels vía URL externa**, lo que implica dependencia de conexión a internet y de la disponibilidad de ese CDN en producción. Se recomienda descargar y alojar localmente esos recursos antes de publicar el sitio.
- No existe favicon, `robots.txt`, `sitemap.xml` ni metaetiquetas SEO/Open Graph.
- No hay manejo de accesibilidad adicional más allá de los `alt` en imágenes (sin `aria-*` en el menú desplegable).

## 8. Cómo ejecutar el proyecto localmente

Al no requerir build, alcanza con servir los archivos estáticos:

**Opción A — abrir directamente:**
Abrir `index.html` en el navegador (algunas rutas relativas funcionan bien vía `file://`, pero se recomienda usar un servidor local para evitar restricciones del navegador).

**Opción B — servidor local simple (recomendado):**
```bash
# Con Python
python -m http.server 8000

# Con Node (npx, sin instalación previa)
npx serve .
```
Luego abrir `http://localhost:8000`.

## 9. Cómo agregar una nueva página

1. Crear el archivo `.html` dentro de `src/pages/`.
2. Copiar el bloque `<header><nav>...</nav></header>` desde una página existente y agregar el nuevo enlace en **todas** las páginas (incluido `index.html`).
3. Enlazar `../style/main.css` y, si corresponde, un CSS propio `../style/<nombre>.css`.
4. Usar las clases del sistema de diseño existentes (`.dos-columnas`, `.tarjeta`, `.boton`, etc.) antes de crear estilos nuevos, para mantener consistencia visual.

## 10. Despliegue

Al ser un sitio 100% estático, puede desplegarse en cualquier hosting de archivos estáticos (GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3, etc.) sin pasos de build. Antes de desplegar a producción se recomienda resolver los puntos listados en la sección [7. Estado actual / pendientes conocidos](#7-estado-actual--pendientes-conocidos), en particular alojar localmente las imágenes externas y completar `main.js` para el formulario de contacto.
