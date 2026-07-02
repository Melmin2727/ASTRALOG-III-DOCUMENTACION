# Fase 2 — Describir el Proceso de Desarrollo en Detalle

**Duración**: 3 días · **Entregables**: Registro de Entrevistas · Registro de Evidencias ·
Papeles de Trabajo de Auditoría.

## Objetivo de la fase

Recopilar, mediante entrevistas y revisión documental/técnica, la evidencia necesaria para
describir en detalle cómo se ejecutó el ciclo de vida de desarrollo del proyecto AstraLog, en
todas sus áreas: gestión del proyecto, requisitos, arquitectura, desarrollo, base de datos,
seguridad, implementación, documentación y mantenimiento.

## Actividades realizadas

- **Entrevistas** al equipo de desarrollo (CoreSystems Solutions) sobre la aplicación de
  Scrum, CMMI, gestión de requisitos, prácticas de codificación y despliegue.
- **Revisión de documentación** entregada por el equipo del proyecto: Project Charter,
  Informe del Proyecto, documentación arquitectónica (modelo C4).
- **Revisión de herramientas** utilizadas durante el desarrollo: repositorios GitHub
  (control de versiones y Pull Requests), Jira (gestión ágil), SonarCloud (análisis estático
  de calidad), Swagger/OpenAPI (documentación de API) y K6 (pruebas de rendimiento).
- **Revisión de la arquitectura monolítica modular** declarada, contrastándola con la
  evidencia de código disponible.
- **Recolección de evidencias** (EV-001 a EV-034 aproximadamente, según la numeración de la
  Matriz de Hallazgos) organizadas por área auditada.

## Áreas evaluadas (según el Checklist SDLC adaptado)

| Área | Aspectos revisados |
|---|---|
| Gestión del Proyecto | Planificación y seguimiento mediante Scrum y Jira |
| Gestión de Requisitos | Documentación de requisitos mediante historias de usuario, trazabilidad |
| Arquitectura del Sistema | Coherencia entre el modelo C4 documentado y la implementación |
| Desarrollo del Software | Uso de GitHub, control de versiones, revisión mediante Pull Requests |
| Seguridad | Mecanismos de autenticación, autorización y gestión de roles |
| Documentación | Suficiencia y nivel de detalle de manuales técnicos y funcionales |
| Base de Datos | Consistencia de la estructura de MySQL con los requisitos funcionales |
| Implementación / DevOps | Procedimientos de instalación, ausencia de CI/CD automatizado |
| Mantenimiento | Existencia de un procedimiento formal de gestión de incidencias |

## Resultado de la fase

Se conformaron los **Papeles de Trabajo de Auditoría**, que consolidan las evidencias (EV-xxx)
referenciadas por cada hallazgo, sirviendo de base directa para la Fase 3 — Evaluación y
Reporte (ver [Matriz de Hallazgos](../hallazgos/matriz-hallazgos.md) y
[Matriz de Riesgos](../hallazgos/matriz-riesgos.md)).
