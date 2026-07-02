# Objetivos y alcance

## 5. Objetivo general

Realizar una auditoría formal del Ciclo de Vida del Desarrollo de Software (SDLC) del
proyecto AstraLog, mediante la evaluación sistemática de sus procesos, controles,
documentación, arquitectura y entregables, con el fin de determinar el grado de cumplimiento
de los estándares **ISO/IEC 12207**, **ISO/IEC 25010**, **CMMI-DEV** y las buenas prácticas
de Ingeniería de Software, identificar riesgos y oportunidades de mejora, y emitir
recomendaciones que contribuyan a garantizar la calidad, seguridad, mantenibilidad y
confiabilidad del sistema implementado para ASTRAMACO III.

## 6. Objetivos específicos

| Código | Objetivo |
|---|---|
| OE1 | Metodología y Gestión del Proyecto: evaluar si el proyecto fue planificado, ejecutado y controlado adecuadamente (Scrum, CMMI, gestión de cambios, seguimiento de actividades). |
| OE7 | Mantenimiento y Soporte: verificar procedimientos de gestión de incidencias, control de versiones, corrección de errores y actualizaciones. |
| OE8 | Documentación: evaluar si la documentación técnica, funcional, arquitectónica y de usuario es suficiente, coherente y accesible. |
| OE9 | Seguridad: verificar la efectividad de los mecanismos de autenticación, autorización, gestión de roles, protección de datos y trazabilidad. |
| OE10 | Calidad del Producto: evaluar las características de calidad conforme al modelo ISO/IEC 25010 (funcionalidad, fiabilidad, usabilidad, eficiencia, mantenibilidad, seguridad). |

!!! note "Numeración de objetivos"
    El documento fuente extraído contiene explícitamente los objetivos OE1, OE7, OE8, OE9 y
    OE10; los objetivos OE2–OE6 no fueron recuperables del texto plano del PDF en la
    extracción realizada. Se documentan únicamente los objetivos con evidencia textual
    directa, sin inventar el contenido de los objetivos faltantes.

## 7. Alcance de la auditoría

La auditoría SDLC del proyecto AstraLog abarca las siguientes áreas:

### 7.1 Gestión del Proyecto
Definición y aprobación de objetivos/alcance/entregables, conformación del equipo,
planificación y seguimiento de actividades, aplicación de Scrum y CMMI, gestión de cambios e
incidencias, evidencia de reuniones y cumplimiento de hitos, participación de stakeholders.

### 7.3 Diseño
Especificación funcional, coherencia entre requisitos y arquitectura C4, diseño del modelo
de datos, especificación de componentes/módulos/interfaces, requisitos de seguridad,
rendimiento y auditoría en el diseño, roles y permisos, consistencia entre diagramas y
código.

### 7.4 Desarrollo
Estándares de programación, entornos de desarrollo/pruebas/despliegue, buenas prácticas y
principios de diseño, control de versiones y gestión de configuración, revisión de código y
análisis estático, documentación técnica de construcción, manejo de errores/excepciones/logs.

### 7.6 Implementación
Plan de implementación y despliegue, procedimientos de instalación/configuración, creación
de la base de datos MySQL, capacitación de usuarios, validación y aceptación por
stakeholders, respaldo y recuperación, configuración de entornos, incidencias durante la
implementación.

### 7.7 Mantenimiento
Procedimientos de gestión de incidencias posteriores, mantenimiento correctivo/preventivo/
evolutivo, control de versiones, documentación de modificaciones.

### 7.8 Documentación
Completitud y calidad de la documentación, control de versiones y accesibilidad,
consistencia entre documentación e implementación real.

### 7.9 Seguridad
Mecanismos de autenticación y generación de tokens JWT, control de acceso basado en roles,
protección y confidencialidad de la información, trazabilidad mediante auditoría, seguridad
en la comunicación frontend/móvil/backend, respaldo de la base de datos MySQL, gestión de
vulnerabilidades y análisis estático.

### 7.10 Calidad
Funcionalidad conforme a requisitos, fiabilidad, usabilidad, eficiencia, mantenibilidad,
seguridad y portabilidad, conforme al modelo ISO/IEC 25010.

## Exclusiones del alcance

- Auditoría financiera, contable o administrativa del proyecto de desarrollo.
- Evaluación de proveedores externos de software, servicios tecnológicos o infraestructura
  de terceros.
- Actividades de mantenimiento, soporte y operación del sistema posteriores a la
  finalización de la auditoría y del plan de mejora propuesto.
