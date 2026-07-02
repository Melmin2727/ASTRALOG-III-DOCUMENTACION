# Informe Final de Auditoría

**Código**: IFA-SDLC-ASTRALOG-001 · **Versión**: 1.0 · **Estado**: Aprobado

## Resumen ejecutivo

Como resultado de la Auditoría del Ciclo de Vida del Desarrollo de Software (SDLC)
realizada al proyecto AstraLog, desarrollado para ASTRAMACO III, se evaluó el grado de
cumplimiento de los procesos de desarrollo, documentación, arquitectura, seguridad, calidad
y gestión del proyecto, tomando como referencia **ISO/IEC 12207:2017**, **ISO/IEC 25010**,
**CMMI-DEV** y el Checklist SDLC. Se concluye que el proyecto presenta un **adecuado nivel
de cumplimiento** de las buenas prácticas de Ingeniería de Software.

## Alcance evaluado

Gestión del Proyecto · Gestión de Requisitos · Diseño y Arquitectura · Desarrollo del
Software · Gestión de Configuración · Control de Versiones · Calidad y Pruebas · Seguridad ·
Implementación · Mantenimiento · Documentación Técnica y Funcional.

No se evaluó el rendimiento de la infraestructura física ni aspectos financieros del
proyecto.

## Documentos y artefactos revisados

Project Charter de Auditoría · Registro de Entrevistas · Registro de Evidencias · Papeles de
Trabajo · Matriz de Hallazgos · Matriz de Riesgos · documentación de requisitos · diagramas
C4 y UML · Manual Técnico · Manual de Usuario · repositorio GitHub · tablero Jira · reportes
SonarCloud · base de datos MySQL.

## Resultados

### Resumen de hallazgos

| Clasificación | Cantidad |
|---|---|
| Conformidades | 6 |
| Oportunidades de Mejora | 3 |
| No Conformidades Menores | 1 |
| No Conformidades Mayores | 0 |
| **Total** | **10** |

**Principales conformidades**: gestión adecuada del proyecto mediante Scrum · correcta
gestión de requisitos · arquitectura coherente con la implementación · uso adecuado del
control de versiones mediante GitHub · implementación correcta de mecanismos de
autenticación y autorización · organización adecuada de la base de datos MySQL.

**Oportunidades de mejora**: fortalecer la documentación técnica · formalizar el proceso de
mantenimiento · implementar un proceso de integración y despliegue continuo (CI/CD).

**No conformidad identificada**: documentación limitada de pruebas unitarias automatizadas.

### Resultados de la evaluación de riesgos

| Nivel | Cantidad |
|---|---|
| Alto | 1 |
| Medio | 5 |
| Bajo | 2 |

El **riesgo prioritario** corresponde a la limitada documentación de pruebas unitarias
automatizadas, que podría dificultar la detección temprana de defectos en futuras versiones
del sistema. Ver el detalle completo en [Matriz de Hallazgos](matriz-hallazgos.md) y
[Matriz de Riesgos](matriz-riesgos.md).

## Conclusiones

- El proyecto AstraLog presenta un adecuado nivel de madurez en la aplicación de prácticas
  de Ingeniería de Software.
- La metodología Scrum permitió una gestión organizada del proyecto y un seguimiento
  continuo de las actividades.
- La arquitectura monolítica modular implementada mantiene consistencia con la
  documentación técnica desarrollada.
- Los mecanismos de autenticación, autorización y control de acceso ofrecen un nivel
  adecuado de protección de la información del sistema.
- El uso de GitHub, Jira y SonarCloud evidencia la adopción de herramientas modernas para el
  desarrollo y aseguramiento de la calidad.
- Las oportunidades de mejora identificadas no comprometen la operación actual del sistema,
  pero su implementación fortalecerá la mantenibilidad, calidad y evolución futura.

## Recomendaciones

1. Documentar de manera formal las pruebas unitarias automatizadas.
2. Implementar una estrategia de Integración Continua y Despliegue Continuo (CI/CD).
3. Formalizar el procedimiento para la gestión de incidencias y mantenimiento.
4. Actualizar periódicamente la documentación técnica y arquitectónica.
5. Mantener revisiones periódicas de seguridad y calidad del código utilizando herramientas
   de análisis estático.
6. Continuar aplicando las buenas prácticas de Scrum, control de versiones y revisiones de
   código en futuras versiones del sistema.

## Opinión del equipo auditor

Con base en la evidencia revisada, el equipo auditor emite una **opinión favorable**
respecto al cumplimiento de los procesos del SDLC del proyecto AstraLog. Las observaciones
identificadas corresponden principalmente a oportunidades de mejora y una no conformidad
menor que no compromete la funcionalidad ni la seguridad del sistema.

Estas cuatro recomendaciones directamente vinculadas a hallazgos con acción correctiva
(pruebas unitarias, CI/CD, gestión de incidencias, documentación técnica) fueron formalizadas
en el [Plan de Acción Correctiva](../fases/plan-accion-correctiva.md).
