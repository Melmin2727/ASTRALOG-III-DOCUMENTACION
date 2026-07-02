# Documentación AstraLog III (MkDocs Material)

Este proyecto contiene la documentación técnica oficial del ecosistema **AstraLog III** (ASTRAMACO III), construida con [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

La documentación cubre:

- Arquitectura general y modular del sistema.
- Backend (Spring Boot): capas, modelo de dominio, seguridad/JWT, auditoría, manejo de errores y configuración.
- Referencia de API estilo OpenAPI para cada endpoint real.
- Frontend (Angular): arquitectura, routing/guards y servicios.
- Aplicación móvil (Android/Kotlin/Compose): arquitectura MVVM, pantallas y navegación.
- Página publicitaria estática.
- Manuales de usuario y de desarrollador.
- Guías de instalación y despliegue.
- Anexo de discrepancias entre el código fuente real y la documentación de referencia del proyecto.

## Requisitos

- Python 3.9 o superior (recomendado 3.11+).
- `pip` actualizado.

Todas las dependencias y sus versiones están fijadas en [`requirements.txt`](requirements.txt), usando exclusivamente **versiones estables** (sin Alpha/Beta/RC), verificadas como compatibles entre sí al momento de generar este proyecto.

## Instalación y ejecución local

=== "Linux / macOS"

    ```bash
    python -m venv .venv
    source .venv/bin/activate

    python -m pip install --upgrade pip
    pip install -r requirements.txt

    mkdocs serve
    ```

=== "Windows (cmd / PowerShell)"

    ```bat
    python -m venv .venv
    .venv\Scripts\activate

    python -m pip install --upgrade pip
    pip install -r requirements.txt

    mkdocs serve
    ```

Luego abrir `http://127.0.0.1:8003/` en el navegador. El servidor de desarrollo de MkDocs recarga automáticamente al guardar cambios en `docs/` o `mkdocs.yml`.

## Build de producción

```bash
mkdocs build
```

Genera el sitio estático en `site/`, listo para publicarse en cualquier hosting estático.

## Estructura del proyecto

```
.
├── mkdocs.yml          # Configuración de MkDocs + Material
├── requirements.txt     # Dependencias Python fijadas
├── README.md             # Este archivo
└── docs/
    ├── index.md
    ├── arquitectura/
    ├── backend/
    ├── api/
    ├── frontend/
    ├── movil/
    ├── manuales/
    ├── anexos/
    ├── pagina-publicitaria.md
    ├── instalacion.md
    └── despliegue.md
```

## Despliegue del sitio de documentación

Este sitio (no la aplicación AstraLog III en sí, sino esta documentación) puede publicarse en:

- **GitHub Pages:** `mkdocs gh-deploy` (requiere que el proyecto esté en un repositorio Git con remoto configurado).
- **GitLab Pages / Netlify / Vercel:** usar `mkdocs build` como comando de build y `site/` como directorio de publicación.
- **Servidor propio (Linux/Windows):** copiar el contenido de `site/` (tras `mkdocs build`) a la raíz servida por Nginx/Apache/IIS.

## Notas sobre las versiones de dependencias

| Paquete | Versión fijada | Motivo |
|---|---|---|
| `mkdocs` | `1.6.1` | Última versión estable al momento de generación. |
| `mkdocs-material` | `9.7.6` | Última versión estable de Material for MkDocs. |
| `mkdocs-minify-plugin` | `0.8.0` | Minifica el HTML generado; mantenido activamente. |
| `mkdocs-git-revision-date-localized-plugin` | `1.5.3` | Fecha de última modificación por página, basada en el historial de Git. |
| `mkdocs-glightbox` | `0.5.2` | Lightbox para imágenes/diagramas. |
| `pymdown-extensions` | `>=10.14,<12` | Habilita admonitions, pestañas, emoji, tareas y los *superfences* usados para renderizar diagramas Mermaid. |

Mermaid se renderiza de forma nativa por Material for MkDocs a través de `pymdownx.superfences` (no requiere un plugin adicional de Mermaid).

Si el repositorio se publica con Git, se recomienda habilitar `enable_creation_date` del plugin de fecha de revisión (ya configurado) para mostrar tanto la fecha de creación como la de última modificación de cada página.
