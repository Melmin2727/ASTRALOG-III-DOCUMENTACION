# ASTRALOG-III-DOCUMENTACION

Documentación completa del proyecto **AstraLog III** (ASTRAMACO III), generada a partir del
análisis del código fuente de los cuatro repositorios oficiales y de la documentación
funcional entregada (Informe del Proyecto, Project Charter y Auditoría SDLC), siguiendo el
código fuente como fuente de verdad en caso de diferencias.

Este paquete contiene **tres proyectos MkDocs independientes**, cada uno con su propio
entorno, dependencias y sitio navegable:

```text
ASTRALOG-III-DOCUMENTACION/
│
├── DOCUMENTACION-TECNICA/     Documentación técnica del sistema AstraLog
│                              (arquitectura, backend, frontend, app móvil, página publicitaria)
│
├── PROJECT-CHARTER/           Project Charter de la Auditoría SDLC
│
├── AUDITORIA-SDLC/            Auditoría SDLC completa (4 fases, hallazgos, riesgos,
│                              plan de acción correctiva, acta de cierre)
│
├── README_GENERAL.md          Este archivo
└── LICENSE
```

## Repositorios oficiales analizados

| Componente | Repositorio | Rama oficial |
|---|---|---|
| Backend | `Backend-AstramacoIII` | `final-trabajo` |
| Frontend Web | `Frontend-AstramacoIII` | `entrega-final` |
| Aplicación Móvil | `AppAstraLog-AstramacoIII` | `entrega-final` |
| Página Publicitaria | `PaginaPublicitaria-AstramacoIII` | `main` |

## Cómo ejecutar cada proyecto

Cada uno de los tres proyectos (`DOCUMENTACION-TECNICA/`, `PROJECT-CHARTER/`,
`AUDITORIA-SDLC/`) es un proyecto MkDocs **independiente y autocontenido**: tiene su propio
`mkdocs.yml`, `requirements.txt`, `README.md`, `.gitignore`, `pyproject.toml` y workflow de
despliegue (`.github/workflows/deploy.yml`).

Para ejecutar cualquiera de los tres, entra a su carpeta y sigue los pasos de su propio
`README.md`. En resumen (ejemplo con `DOCUMENTACION-TECNICA`):

### Windows
```powershell
cd DOCUMENTACION-TECNICA
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
mkdocs serve
```

### Linux / macOS
```bash
cd DOCUMENTACION-TECNICA
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
mkdocs serve
```

Repite el mismo procedimiento dentro de `PROJECT-CHARTER/` y `AUDITORIA-SDLC/` (en entornos
virtuales separados, o reutilizando el mismo entorno ya que las tres comparten exactamente
las mismas versiones de dependencias).

## Dependencias y versiones (idénticas en los tres proyectos)

| Paquete | Versión |
|---|---|
| Python | ≥ 3.9 |
| mkdocs | 1.6.1 |
| mkdocs-material | 9.5.49 |
| pymdown-extensions | 10.12 |

Todas son versiones **estables, oficiales y con soporte vigente** (sin alpha/beta/RC),
verificadas mediante `mkdocs build --strict` en un entorno limpio para los tres proyectos,
sin errores ni advertencias.

## Discrepancias detectadas

Cada uno de los tres proyectos incluye su propia página **"Discrepancias detectadas"**,
documentando las diferencias encontradas entre la documentación funcional y el código
fuente (por ejemplo, la tecnología declarada para la aplicación móvil en el Project Charter
frente a la implementación real en Kotlin/Android nativo). En todos los casos, el código
fuente fue tratado como fuente de verdad.

## Origen de la documentación

Generada mediante análisis directo de:

1. Los cuatro repositorios oficiales de GitHub, en sus ramas oficiales indicadas arriba.
2. `DOCUMENTOS/Informe.pdf` — Informe del Proyecto.
3. `DOCUMENTOS/Project Charter.pdf`.
4. `DOCUMENTOS/Auditoría SDLC.zip` — entregables de las cuatro fases de auditoría.

No se documentaron funcionalidades, endpoints, componentes ni relaciones sin evidencia
directa en el código fuente o en la documentación oficial del proyecto.
