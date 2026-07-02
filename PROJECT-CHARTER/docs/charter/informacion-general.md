# Información general y antecedentes

## 1. Información general del proyecto

| Campo | Detalle |
|---|---|
| Nombre del Proyecto | AstraLog – Solución Logística Centralizada |
| Código del Proyecto | AUD-SDLC-ASTRA-LOGIII-2026-001 |
| Versión del documento | 1.0 |
| Fecha de elaboración | 23 de junio de 2026 |
| Fecha de inicio estimada | 23 de junio de 2026 |
| Fecha de cierre estimada | 09 de septiembre de 2026 |
| Patrocinador | Universidad Peruana Unión — Facultad de Ingeniería y Arquitectura — Gerencia de ASTRAMACO III |
| Auditor líder | Elvis Jesus Apaza Yucra |
| Equipo auditor | Anthony Kelman Mamani Vargas, Yunior Benito Quispe Quispe, Jeimy Paul Ramos Coaquira |
| Clasificación | Proyecto de Desarrollo e Implementación de Sistema de Información Logístico |
| Sistema auditado | AstraLog – Solución Logística Centralizada |
| Organización auditada | ASTRAMACO III |

## 2. Antecedentes

### 2.1 Descripción del sistema AstraLog

AstraLog es un Sistema Centralizado de Gestión Logística y Comercial desarrollado por el
startup **CoreSystems Solutions**, conformado por estudiantes del VII ciclo de la carrera de
Ingeniería de Sistemas de la Universidad Peruana Unión, en el contexto del curso de
Ingeniería de Software II, bajo la supervisión del Ing. Ruben Roque Sucari.

El sistema fue concebido para dar solución a las necesidades operativas de la empresa
ASTRAMACO III, dedicada al transporte y distribución de materiales de construcción, la cual
presentaba dificultades en la gestión de ventas, asignación de transportistas y control
logístico debido a procesos manuales y poco estandarizados.

### 2.2 Arquitectura y tecnología

El sistema adopta una **arquitectura monolítica modular**, documentada mediante el modelo
C4 (Contexto, Contenedores y Componentes). Componentes principales declarados en el
Project Charter:

- **Aplicación Web (Frontend)**: gestión de pedidos, monitoreo de operaciones,
  administración de usuarios y consulta de reportes.
- **Aplicación Móvil**: recepción de hojas de ruta, actualización de estados de entrega y
  registro de evidencias de cumplimiento por parte de los transportistas.
- **Backend Monolítico (Java Spring Boot)**: procesamiento de las reglas de negocio.
- **Módulo de Autenticación**: validación de credenciales, tokens JWT y control de sesiones.
- **Módulo de Usuarios**: gestión de perfiles, roles y permisos.
- **Módulo de Ventas y Pedidos**: registro de pedidos, cálculo de tarifas y asignación.

Tecnologías declaradas en el criterio de auditoría (sección 9 del documento original):

- Arquitectura monolítica modular.
- Backend Java Spring Boot.
- Frontend Angular.
- Aplicación móvil **Flutter** *(ver nota de discrepancia más abajo)*.
- Base de datos MySQL.
- Autenticación mediante JWT.
- Documentación de API con Swagger/OpenAPI.

!!! danger "Discrepancia: tecnología móvil"
    El Project Charter declara **Flutter** para la aplicación móvil. El repositorio oficial
    (`AppAstraLog-AstramacoIII`, rama `entrega-final`) está construido en **Android nativo
    con Kotlin y Jetpack Compose**, sin artefactos de Flutter/Dart. Ver
    [Discrepancias detectadas](../discrepancias.md).

### 2.3 Contexto de la auditoría

La auditoría tiene como finalidad evaluar el cumplimiento de los procesos, prácticas y
controles aplicados durante el desarrollo del sistema AstraLog para ASTRAMACO III,
considerando buenas prácticas de Ingeniería de Software y estándares como
**ISO/IEC 12207**, **ISO/IEC 25010** y **CMMI-DEV**, con el propósito de verificar la
calidad, trazabilidad y conformidad del ciclo de vida del software.
