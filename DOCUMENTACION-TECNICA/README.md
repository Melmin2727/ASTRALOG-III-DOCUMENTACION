# Documentación Técnica — AstraLog III

Sitio de documentación técnica oficial del sistema **AstraLog III**, construido con
[MkDocs](https://www.mkdocs.org/) y el tema [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## Requisitos

- Python 3.9 o superior.

## Instalación y ejecución local

### Windows

```powershell
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
mkdocs serve
```

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
mkdocs serve
```

Luego abre `http://127.0.0.1:8000` en tu navegador.

## Compilación para producción

```bash
mkdocs build
```

El sitio estático se genera en `site/`.

## Despliegue en GitHub Pages

El workflow `.github/workflows/deploy.yml` compila y publica automáticamente el sitio en
GitHub Pages en cada push a `main`, mediante `mkdocs gh-deploy`.

## Estructura del proyecto

```text
DOCUMENTACION-TECNICA/
├── docs/                  # Contenido en Markdown
│   ├── arquitectura/
│   ├── backend/
│   ├── frontend/
│   ├── movil/
│   ├── pagina-publicitaria/
│   ├── index.md
│   └── discrepancias.md
├── mkdocs.yml              # Configuración de MkDocs Material
├── requirements.txt
├── pyproject.toml
├── .gitignore
└── .github/workflows/deploy.yml
```

## Fuente de la documentación

Generada a partir del análisis del código fuente de los repositorios oficiales de AstraLog
III (backend, frontend, aplicación móvil y página publicitaria) contrastado con el Informe
del Proyecto y el Project Charter. Ver [Discrepancias detectadas](docs/discrepancias.md) para
las diferencias identificadas entre documentación funcional y código.
