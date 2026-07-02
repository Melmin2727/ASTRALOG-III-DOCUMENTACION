# Project Charter — Auditoría SDLC de AstraLog III

Sitio de documentación del Project Charter de la Auditoría del Ciclo de Vida del Desarrollo
de Software (SDLC) del proyecto AstraLog III, construido con MkDocs Material.

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

## Compilación

```bash
mkdocs build
```

## Despliegue

El workflow `.github/workflows/deploy.yml` publica el sitio en GitHub Pages en cada push a
`main`.

## Fuente

Basado en `DOCUMENTOS/Project Charter.pdf` (24 páginas), contrastado con el código fuente de
los repositorios oficiales. Ver [Discrepancias detectadas](docs/discrepancias.md).
