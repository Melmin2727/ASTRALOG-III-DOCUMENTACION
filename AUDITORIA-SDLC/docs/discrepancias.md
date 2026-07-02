# Discrepancias detectadas

## 1. Nombre de la organización: "ASTROMACO" (documentos de auditoría) vs. "AstraLog" / "Astramaco" (resto del proyecto)

Todos los documentos de la carpeta `Auditoria SDLC.zip` (Plan de Auditoría, Matriz de
Hallazgos, Matriz de Riesgos, Informe Final, Plan de Acción Correctiva, Acta de Cierre) usan
consistentemente el nombre **"EMPRESA ASTROMACO III"** en sus títulos, mientras que el
Informe del Proyecto y el Project Charter usan predominantemente **"AstraLog"** como nombre
del producto y **"ASTRAMACO III"** para la organización cliente.

- **Resolución**: se trata de variantes de escritura del mismo nombre, no de una
  inconsistencia de alcance — los cuatro entregables (auditoría, informe, charter y código)
  describen consistentemente al mismo proyecto, equipo y organización cliente. Se usó
  **AstraLog** para el producto y **ASTRAMACO III** para la organización en toda esta
  documentación, siguiendo la forma predominante en el Informe y el Project Charter.

## 2. H-005 (Seguridad) sin texto completo en la extracción del PDF

En la extracción de texto de `MATRIZ DE HALLAZGOS...pdf`, el hallazgo H-005 (Seguridad)
quedó parcialmente cortado en el salto de página entre la página 4 y la página 5 del
documento original; solo se recuperó el cierre de su descripción ("...de errores en futuras
modificaciones") y su clasificación no aparece explícita junto al resto de la fila en el
texto extraído.

- **Resolución**: se infirió la clasificación de H-005 como **Conformidad** a partir de tres
  fuentes convergentes dentro del propio documento: (a) el resumen numérico de la sección 4
  del mismo documento (6 conformidades en total, de las cuales H-001 a H-004 y H-008 están
  explícitamente confirmadas como conformidad, dejando a H-005 como la sexta), (b) la Matriz
  de Riesgos, que vincula el riesgo R-006 ("Configuración incorrecta de roles y permisos") a
  H-005 con probabilidad baja, y (c) el Informe Final de Auditoría, que lista expresamente
  como conformidad la "implementación correcta de mecanismos de autenticación y
  autorización". No se inventó contenido adicional sobre H-005 más allá de lo que estas tres
  fuentes permiten reconstruir con consistencia.

## 3. Relación entre la Auditoría SDLC y el código fuente de los cuatro repositorios

Los documentos de Auditoría SDLC describen la revisión de "repositorio GitHub", "tablero
Jira" y "reportes SonarCloud", pero no detallan cuáles evidencias específicas (EV-xxx)
corresponden a cuáles archivos o commits concretos de los cuatro repositorios oficiales
analizados en el proyecto **DOCUMENTACION-TECNICA**.

- **Resolución**: no se intentó reconstruir una correspondencia evidencia-por-evidencia que
  no está documentada explícitamente. Donde el código fuente confirma directamente una
  conformidad reportada por la auditoría (por ejemplo, H-005 / mecanismos de autenticación
  JWT, verificable en `SecurityConfig` y `JwtFilter` del backend), se señaló la coincidencia
  en el proyecto **DOCUMENTACION-TECNICA → Backend → Seguridad**.

---

No se detectaron discrepancias entre los documentos de Auditoría SDLC entre sí (Plan de
Auditoría, Matriz de Hallazgos, Matriz de Riesgos, Informe Final, Plan de Acción Correctiva y
Acta de Cierre son internamente consistentes en cifras, códigos de hallazgo y conclusiones).
