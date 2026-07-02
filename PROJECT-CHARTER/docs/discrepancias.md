# Discrepancias detectadas

## 1. Portada del documento con el título de otro proyecto de curso

La portada de `Project Charter.pdf` incluye el título "PROYECTO SYS PROFAR — Sistema de
Gestión Farmacéutica — Farmacia RUBRI E.I.R.L." junto con el nombre correcto del producto
("AstraLog – Solución Logística Centralizada"). El resto del documento (24 páginas) se
refiere consistentemente a AstraLog y ASTRAMACO III.

- **Resolución**: se documentó únicamente el contenido sustantivo, referido a AstraLog. La
  línea de portada con el nombre de otro proyecto no se reprodujo en este sitio.

## 2. Tecnología de la aplicación móvil: Flutter (Charter) vs. Android nativo/Kotlin (código)

La sección 9 (Criterios de Auditoría) del Project Charter identifica **Flutter** como
tecnología de la aplicación móvil. El repositorio oficial `AppAstraLog-AstramacoIII`
(rama `entrega-final`) es un proyecto **Android nativo en Kotlin** (Gradle Kotlin DSL,
Jetpack Compose, Retrofit 2.9.0, OkHttp 4.10.0), sin ningún artefacto de Flutter o Dart.

- **Impacto en la auditoría**: este es exactamente el tipo de hallazgo que la propia
  auditoría busca detectar (riesgo **R-03**, "información inconsistente entre la
  documentación y la implementación del sistema", del Project Charter).
- **Resolución**: se documentó el stack declarado en el Project Charter tal como aparece en
  el documento original (para preservar la trazabilidad del criterio de auditoría), y se
  señaló explícitamente la discrepancia frente al código fuente, que se toma como fuente de
  verdad para la documentación técnica del sistema (ver el proyecto de documentación
  **DOCUMENTACION-TECNICA → Aplicación Móvil**).

## 3. Inconsistencia interna de fechas de cierre

La ficha de "Información General del Proyecto" (sección 1) indica una fecha de cierre
estimada del **09 de septiembre de 2026**, mientras que el cronograma de alto nivel
(sección 13) programa el cierre de todas las fases de la auditoría para el
**29 de junio de 2026**. Ambas fechas provienen del mismo documento original.

- **Resolución**: se transcriben ambas fechas tal como constan en el documento fuente, sin
  intentar resolver la diferencia, ya que se trata de una inconsistencia interna del propio
  Project Charter y no de una diferencia entre documentación funcional y código fuente (que
  es el tipo de discrepancia que este flujo de trabajo prioriza resolver a favor del código).

## 4. Contenido no recuperable de la extracción de texto del PDF

Algunas tablas y listas del documento original (parte de la tabla de "Stakeholders del
Sistema AstraLog" en la sección 12.2, y los ítems 3–4 de "Criterios de Aceptación" en la
sección 17) quedaron truncadas en los saltos de página durante la extracción de texto del
PDF. Se documentó únicamente el contenido con evidencia textual directa, señalando
explícitamente los puntos donde el documento fuente no pudo recuperarse por completo, en
lugar de inferir o completar dicho contenido.

---

Estas discrepancias son consistentes con — y en el caso del punto 2, coinciden textualmente
con — las señaladas en el proyecto de documentación **DOCUMENTACION-TECNICA**.
