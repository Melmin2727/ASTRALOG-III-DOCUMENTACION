# Discrepancias detectadas

Conforme a la regla de consistencia del proyecto, cuando existe una diferencia entre la
documentación funcional (Informe, Project Charter, Auditoría SDLC) y el código fuente de los
repositorios oficiales, **el código fuente tiene prioridad**. Esta sección documenta cada
diferencia encontrada durante el análisis.

## 1. Portada del Project Charter con título de otro proyecto de curso

**Origen**: `DOCUMENTOS/Project Charter.pdf`, página 1.

La portada del documento incluye el título "PROYECTO SYS PROFAR — Sistema de Gestión
Farmacéutica — Farmacia RUBRI E.I.R.L." junto con el nombre correcto del producto
("AstraLog – Solución Logística Centralizada"). El resto del documento (24 páginas) se
refiere consistentemente a AstraLog y ASTRAMACO III.

- **Causa probable**: reutilización de una plantilla de curso de otro grupo/proyecto sin
  actualizar completamente la portada.
- **Resolución**: se utilizó el contenido sustantivo del documento (secciones 1 en
  adelante), que sí corresponde a AstraLog / ASTRAMACO III, para el proyecto de
  documentación **PROJECT-CHARTER**. La línea de la portada con el nombre de otro proyecto
  no fue reproducida en la documentación generada.

## 2. Variantes del nombre de la organización

**Origen**: comparación entre nombres de repositorio, paquetes Java y documentación.

Se observan tres formas del nombre de la empresa/proyecto a lo largo de las fuentes:

| Forma | Dónde aparece |
|---|---|
| `Astramaco` | Nombres de los cuatro repositorios GitHub y paquete Java `backendastramaco` |
| `AstraLog` | Nombre de producto en Informe y Project Charter, documentación funcional |
| `ASTROMACO` | Archivos de la Auditoría SDLC (informes, actas, matrices) |

- **Resolución**: no se trata de una inconsistencia de alcance (todas las fuentes describen
  el mismo sistema y al mismo equipo de cuatro integrantes), sino de variantes de escritura
  del mismo nombre. En esta documentación se usa **AstraLog** para el nombre del producto y
  **ASTRAMACO III** para la organización cliente, siguiendo la nomenclatura predominante en
  el Informe y el Project Charter, y se señala la variante `Astramaco` al referirse
  explícitamente a nombres de repositorio o paquetes de código.

## 3. Módulos descritos en el Informe sin evidencia directa de código dedicado

**Origen**: `DOCUMENTOS/Informe.pdf`, sección 4.6 (Vista de componentes), que enumera
"Módulo de Ventas", "Módulo de Inventario", "Módulo de Autenticación", "Módulo de Usuarios" y
"Módulo de Auditoría".

- En el código del backend (`final-trabajo`) se confirmaron con evidencia directa:
  **Módulo de Autenticación**, **Módulo de Usuarios** y **Módulo de Auditoría** (ver
  [Arquitectura modular](arquitectura/arquitectura-modular.md)).
- Los conceptos de "Ventas" e "Inventario" mencionados en el Informe se corresponden, en el
  código, con los dominios **Pedido** (ventas/pedidos de transporte) y **Carga** /
  **TipoMaterial** (control de materiales transportados), sin paquetes literalmente llamados
  `ventas` o `inventario`.
- **Resolución**: se documentaron los módulos con los nombres presentes en el código
  (`Pedido`, `Carga`) y se indicó su correspondencia conceptual con lo descrito en el
  Informe, evitando introducir nombres de módulo inexistentes en el código fuente.

## 4. Entidad `Usuario` marcada como `@Deprecated` en el código

**Origen**: `Usuario.java`, anotación `@Deprecated` sobre la clase.

Ni el Informe ni el Project Charter mencionan una migración o reemplazo planificado de la
entidad `Usuario`. El código, sin embargo, la marca explícitamente como obsoleta.

- **Resolución**: se documentó tal como aparece en el código (ver
  [Modelo de datos](backend/modelo-datos.md)), sin inferir ni inventar cuál sería la entidad
  de reemplazo, dado que no hay evidencia de código que lo confirme.

## 5. Tecnología de la aplicación móvil: Flutter (documentado) vs. Android nativo/Kotlin (código)

**Origen**: `DOCUMENTOS/Project Charter.pdf`, sección 9 (Criterios de Auditoría), que lista
el stack tecnológico identificado como "Aplicación móvil Flutter".

El repositorio oficial `AppAstraLog-AstramacoIII` (rama `entrega-final`) es un proyecto
**Android nativo en Kotlin** (Gradle Kotlin DSL, Jetpack Compose, Retrofit), sin artefactos
de Flutter/Dart en ningún punto del árbol de archivos.

- **Causa probable**: el Project Charter de Auditoría fue redactado antes de una decisión
  técnica de cambiar de framework, o describe el stack planeado inicialmente en el Informe
  del Proyecto en lugar del stack final entregado.
- **Resolución**: se documentó la Aplicación Móvil como **Android nativo (Kotlin +
  Jetpack Compose)**, tal como existe en la rama oficial `entrega-final`, que es la fuente de
  verdad (ver [Aplicación Móvil](movil/index.md)). Esta discrepancia también se señala en el
  proyecto de documentación **PROJECT-CHARTER**, ya que afecta directamente a un criterio
  de auditoría declarado.

---

No se detectaron discrepancias adicionales entre el alcance funcional descrito en la
documentación oficial y los componentes verificados en el código fuente de las cuatro ramas
oficiales analizadas.
