# Página Publicitaria

**Repositorio**: `PaginaPublicitaria-AstramacoIII` · **Rama oficial**: `main`

## Propósito

Sitio web estático de presentación institucional de **ASTRAMACO III** — "empresa de
camioneros y volqueteros transportistas de material de construcción en Juliaca, Puno" (según
el `meta description` del propio `index.html`). No implementa lógica de negocio ni se conecta
a la API del backend de AstraLog en el código analizado.

## Estructura

```text
pagina/
├── index.html    # 315 líneas — estructura y contenido
├── main.js       # 14 líneas — scroll suave para anclas del menú
├── styles.css    # 450 líneas — estilos del sitio
└── img/
    └── logo-astramaco.jpeg
```

## Tecnologías

- **HTML5** semántico, sin framework.
- **CSS3** puro (`styles.css`), sin preprocesador ni utilidades tipo Tailwind detectadas.
- **JavaScript vanilla** (`main.js`): un único listener `DOMContentLoaded` que habilita
  desplazamiento suave (`scrollIntoView({ behavior: "smooth" })`) para los enlaces de anclaje
  del menú de navegación.

## Secciones del sitio

| Sección (`id`) | Contenido |
|---|---|
| `inicio` | Hero principal: propuesta de valor y llamados a la acción ("Solicitar atención", "Ver servicios"). |
| `nosotros` | Presentación de la empresa. |
| `servicios` | Servicios ofrecidos de transporte y distribución de materiales. |
| `ubicacion` | Ubicación de la empresa (Juliaca, Puno). |
| `contacto` | Datos de contacto, incluyendo enlace directo `tel:916859627`. |

## Despliegue

Al tratarse de un sitio 100% estático (HTML/CSS/JS sin dependencias de build), es compatible
con cualquier hosting estático (GitHub Pages, Netlify, Vercel, etc.) sin proceso de
compilación adicional.
