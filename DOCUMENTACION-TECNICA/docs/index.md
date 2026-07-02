# AstraLog III — Documentación Técnica

**AstraLog** es el Sistema Centralizado de Gestión Logística y Comercial desarrollado para
**ASTRAMACO III**, empresa dedicada al transporte y distribución de materiales de construcción
(pandereta, arena, piedra, desmonte, entre otros). El sistema fue construido por el startup
académico **CoreSystems Solutions** (Ciclo VII, Ingeniería de Sistemas, Universidad Peruana
Unión) como parte del curso de Ingeniería de Software II.

Esta documentación es generada a partir del análisis directo del código fuente de los cuatro
repositorios oficiales del proyecto y de la documentación funcional entregada (Informe de
Trabajo Final y Project Charter). En caso de diferencias entre el código y los documentos, el
código fuente es la fuente de verdad — ver [Discrepancias detectadas](discrepancias.md).

## Problema que resuelve

ASTRAMACO III gestionaba sus pedidos, transportistas y control logístico mediante procesos
manuales y poco estandarizados, generando ineficiencias en la asignación de transportistas,
el control de cargas y el seguimiento del estado de los pedidos.

## Componentes del sistema

| Componente | Repositorio | Rama oficial | Tecnología |
|---|---|---|---|
| Backend | `Backend-AstramacoIII` | `final-trabajo` | Java 17 · Spring Boot 3.3.5 |
| Frontend Web | `Frontend-AstramacoIII` | `entrega-final` | Angular |
| Aplicación Móvil | `AppAstraLog-AstramacoIII` | `entrega-final` | Android nativo (Kotlin) |
| Página Publicitaria | `PaginaPublicitaria-AstramacoIII` | `main` | HTML5 / CSS3 / JavaScript |

## Mapa de la documentación

- **[Arquitectura](arquitectura/vision-general.md)** — vista general, arquitectura modular y
  flujo completo de una petición a través del sistema.
- **[Backend](backend/index.md)** — módulos, referencia de API REST, modelo de datos y
  seguridad.
- **[Frontend Web](frontend/index.md)** — estructura Angular, páginas y servicios.
- **[Aplicación Móvil](movil/index.md)** — arquitectura Android orientada a transportistas.
- **[Página Publicitaria](pagina-publicitaria/index.md)** — sitio de presentación institucional.
- **[Discrepancias detectadas](discrepancias.md)** — diferencias entre documentación funcional
  y código fuente, con la justificación de la fuente utilizada.

!!! info "Alcance del análisis"
    Este sitio documenta únicamente lo verificable en las ramas oficiales indicadas arriba.
    No se documentan funcionalidades, endpoints, entidades ni diagramas que no tengan
    evidencia directa en el código fuente o en la documentación oficial del proyecto.
