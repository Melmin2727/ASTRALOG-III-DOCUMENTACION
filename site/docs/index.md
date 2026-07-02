# AstraLog III

**AstraLog III** es el ecosistema digital de **ASTRAMACO III**, una empresa de Juliaca (Puno, Perú) dedicada a la intermediación y distribución de materiales de construcción a través de transportistas propios (camioneros y volqueteros).

El sistema está compuesto por cuatro componentes independientes que se comunican entre sí mediante una API REST central:

| Componente | Tecnología principal | Rol |
|---|---|---|
| **Backend** | Spring Boot 3.3.5 (Java 17) | API REST central, lógica de negocio, seguridad y persistencia |
| **Frontend web** | Angular 21 | Panel de administración para gestión de pedidos, transportistas, cargas y documentos |
| **Aplicación móvil** | Android nativo (Kotlin + Jetpack Compose) | App para transportistas: ver perfil, gestionar carga y pedidos asignados |
| **Página publicitaria** | HTML5 + CSS3 + JavaScript | Sitio institucional de presentación de la empresa |

!!! info "Sobre esta documentación"
    Este sitio se generó a partir del análisis directo del código fuente de los cuatro repositorios del proyecto. Cuando el código y la documentación de referencia entregada por el equipo presentaban diferencias, **se priorizó siempre el código fuente real**, y dichas diferencias quedan registradas en [Anexos → Discrepancias detectadas](anexos/discrepancias.md).

## Mapa rápido de la documentación

- **[Arquitectura](arquitectura/general.md)** — Cómo se comunican los componentes, diagramas C4/Mermaid y modelo de base de datos.
- **[Backend](backend/index.md)** — Capas, entidades, seguridad JWT, auditoría y manejo de errores.
- **[API](api/index.md)** — Referencia estilo OpenAPI de todos los endpoints reales expuestos por el backend.
- **[Frontend](frontend/index.md)** — Estructura de la SPA Angular, rutas, guards e interceptores.
- **[Aplicación móvil](movil/index.md)** — Arquitectura MVVM, pantallas y consumo de API en Android.
- **[Página publicitaria](pagina-publicitaria.md)** — Estructura del sitio institucional.
- **[Manuales](manuales/usuario.md)** — Manual de usuario final y manual del desarrollador.
- **[Instalación y despliegue](instalacion.md)** — Cómo levantar cada componente desde cero.

## Glosario de dominio

| Término | Significado en el sistema |
|---|---|
| **Transportista** | Persona registrada como `CAMIONERO` o `VOLQUETERO`, asociada 1 a 1 con un `Usuario`. |
| **Carga** | Inventario de material (`TipoMaterial`) que un transportista de tipo `CAMIONERO` tiene disponible para atender pedidos. |
| **Pedido** | Solicitud de un cliente para el transporte/entrega de un material específico, asignada a un transportista. |
| **Documento personal** | Documento asociado a un transportista (SOAT, revisión técnica, licencia, tarjeta de circulación, DNI). |
| **Auditoría** | Registro histórico de creación/actualización/eliminación de entidades clave del sistema. |
