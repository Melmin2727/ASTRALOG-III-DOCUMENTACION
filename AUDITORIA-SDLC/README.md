# Auditoría SDLC — AstraLog III

Sitio de documentación de la Auditoría del Ciclo de Vida del Desarrollo de Software (SDLC)
del proyecto AstraLog III, construido con MkDocs Material.

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

## Contenido

- Fase 1 — Preparar y Planificar
- Fase 2 — Describir el Proceso de Desarrollo
- Fase 3 — Evaluar y Reportar (Matriz de Hallazgos, Matriz de Riesgos, Informe Final)
- Fase 4 — Seguimiento (Plan de Acción Correctiva, Acta de Cierre)
- Discrepancias detectadas

## Fuente

Basado en los entregables de `DOCUMENTOS/Auditoría SDLC.zip`. Ver
[Discrepancias detectadas](docs/discrepancias.md).
