# Riesgos del proyecto de auditoría

## 15. Riesgos del proyecto de auditoría

| ID | Riesgo | Prob. | Impacto | Nivel | Estrategia de respuesta |
|---|---|---|---|---|---|
| R-01 | Documentación incompleta del proyecto AstraLog | Alta | Alto | **Crítico** | Solicitar toda la documentación disponible; registrar la ausencia de información como hallazgo de auditoría |
| R-02 | Falta de evidencias de pruebas funcionales, integración o rendimiento | Media | Alto | Alto | Revisar repositorios y registros de pruebas; entrevistar al equipo de desarrollo |
| R-03 | Información inconsistente entre la documentación y la implementación del sistema | Media | Alto | Alto | Comparar documentación con arquitectura, código fuente y evidencias técnicas disponibles |
| R-04 | Acceso limitado al repositorio o a los artefactos del proyecto | Media | Medio | Medio | Coordinar con el equipo de desarrollo el acceso oportuno a los recursos |
| R-05 | Cambios en el alcance durante la ejecución de la auditoría | Baja | Alto | Medio | Gestionar cualquier cambio mediante autorización del Auditor Líder; actualizar el plan cuando corresponda |
| R-06 | Disponibilidad limitada de los integrantes para entrevistas o validaciones | Media | Medio | Medio | Programar reuniones con anticipación; usar medios virtuales cuando sea necesario |
| R-07 | Sesgo en la evaluación al tratarse de un proyecto académico desarrollado por el mismo equipo | Baja | Alto | Medio | Aplicar criterios objetivos basados en ISO/IEC 12207, ISO/IEC 25010, CMMI-DEV y el Checklist SDLC |
| R-08 | Pérdida o eliminación accidental de evidencias y papeles de trabajo | Baja | Alto | Medio | Mantener copias de seguridad en un repositorio seguro y controlado |
| R-09 | Retraso en la revisión y aprobación de entregables de auditoría | Media | Medio | Medio | Realizar revisiones periódicas y seguimiento al cronograma establecido |
| R-10 | Errores en la interpretación de la evidencia recopilada | Baja | Alto | Medio | Validar los hallazgos con múltiples fuentes de información |

!!! danger "Riesgo R-03 en retrospectiva"
    El riesgo **R-03** ("información inconsistente entre la documentación y la
    implementación del sistema") es precisamente el que se materializó parcialmente durante
    este análisis: se identificaron diferencias puntuales entre el Project Charter y el
    código fuente (por ejemplo, el framework declarado para la aplicación móvil). Ver
    [Discrepancias detectadas](../discrepancias.md) para el detalle completo, resuelto
    aplicando la estrategia de respuesta ya prevista para R-03: comparación directa con el
    código fuente como fuente de verdad.
