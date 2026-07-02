# Referencia de API

Todos los endpoints están bajo el prefijo `/api`. La documentación interactiva generada por
springdoc está disponible en `/swagger-ui.html` (especificación en `/v3/api-docs`) cuando el
backend está en ejecución.

!!! tip "Fuente de esta referencia"
    Extraída directamente de las anotaciones `@RequestMapping` / `@GetMapping` /
    `@PostMapping` / `@PutMapping` / `@PatchMapping` / `@DeleteMapping` de los controladores
    en la rama `final-trabajo`. No se documentan cuerpos de petición/respuesta que no estén
    modelados explícitamente por DTOs en el código.

## Autenticación — `/api/auth`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/auth/login` | Autentica un usuario (`AuthRequest`) y retorna un token JWT (`AuthResponse`). |

## Usuarios — `/api/usuarios`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/usuarios` | Crear usuario. Requiere rol `ADMIN`. |
| `GET` | `/api/usuarios` | Listar usuarios (activos). |
| `GET` | `/api/usuarios/todos` | Listar todos los usuarios. |
| `GET` | `/api/usuarios/eliminados` | Listar usuarios eliminados lógicamente. |
| `GET` | `/api/usuarios/{id}` | Obtener usuario por id. |
| `GET` | `/api/usuarios/username/{username}` | Buscar usuario por username. |
| `PUT` | `/api/usuarios/{id}` | Actualizar usuario. |
| `PATCH` | `/api/usuarios/{id}/estado` | Cambiar estado (activo/inactivo). |
| `DELETE` | `/api/usuarios/{id}` | Borrado lógico. |
| `PATCH` | `/api/usuarios/{id}/restaurar` | Restaurar usuario eliminado lógicamente. |
| `DELETE` | `/api/usuarios/{id}/permanente` | Borrado físico. |

## Pedidos — `/api/pedidos`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/pedidos` | Crear pedido. |
| `GET` | `/api/pedidos` | Listar pedidos (activos). |
| `GET` | `/api/pedidos/todos` | Listar todos los pedidos. |
| `GET` | `/api/pedidos/eliminados` | Listar pedidos eliminados lógicamente. |
| `GET` | `/api/pedidos/{id}` | Obtener pedido por id. |
| `GET` | `/api/pedidos/me` | Obtener pedidos del usuario autenticado. |
| `PUT` | `/api/pedidos/{id}` | Actualizar pedido. |
| `PATCH` | `/api/pedidos/{id}/estado` | Cambiar `EstadoPedido`. |
| `DELETE` | `/api/pedidos/{id}` | Borrado lógico. |
| `PATCH` | `/api/pedidos/{id}/restaurar` | Restaurar pedido. |
| `DELETE` | `/api/pedidos/{id}/permanente` | Borrado físico. |

## Cargas — `/api/cargas`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/cargas/{transportistaId}` | Registrar carga para un transportista. |
| `PUT` | `/api/cargas/{transportistaId}` | Actualizar carga del transportista. |
| `POST` | `/api/cargas/{transportistaId}/aumentar` | Aumentar cantidad de carga. |
| `GET` | `/api/cargas/{transportistaId}` | Obtener carga activa del transportista. |
| `GET` | `/api/cargas/{id}/detalle` | Obtener detalle de una carga. |
| `GET` | `/api/cargas` | Listar cargas. |
| `GET` | `/api/cargas/transportista/{transportistaId}/historial` | Historial de cargas del transportista. |
| `DELETE` | `/api/cargas/{id}` | Borrado lógico. |
| `PATCH` | `/api/cargas/{id}/restaurar` | Restaurar carga. |
| `DELETE` | `/api/cargas/{id}/permanente` | Borrado físico. |

## Transportistas — `/api/transportistas`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/transportistas` | Crear transportista. |
| `GET` | `/api/transportistas` | Listar transportistas (activos). |
| `GET` | `/api/transportistas/todos` | Listar todos los transportistas. |
| `GET` | `/api/transportistas/eliminados` | Listar eliminados lógicamente. |
| `GET` | `/api/transportistas/{id}` | Obtener por id. |
| `GET` | `/api/transportistas/dni/{dni}` | Buscar por DNI. |
| `GET` | `/api/transportistas/usuario/{usuarioId}` | Buscar por usuario asociado. |
| `GET` | `/api/transportistas/tipo/{tipo}` | Filtrar por `TipoTransporte`. |
| `GET` | `/api/transportistas/{id}/documentos` | Documentos personales del transportista. |
| `GET` | `/api/transportistas/me` | Transportista del usuario autenticado. |
| `PUT` | `/api/transportistas/{id}` | Actualizar transportista. |
| `PATCH` | `/api/transportistas/{id}/estado` | Cambiar `EstadoTransportista`. |
| `DELETE` | `/api/transportistas/{id}` | Borrado lógico. |
| `PATCH` | `/api/transportistas/{id}/restaurar` | Restaurar transportista. |
| `DELETE` | `/api/transportistas/{id}/permanente` | Borrado físico. |

## Documentos personales — `/api/documentos`

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/documentos/{transportistaId}` | Registrar documento personal. |
| `GET` | `/api/documentos/{id}` | Obtener documento por id. |
| `GET` | `/api/documentos/transportista/{id}` | Documentos de un transportista. |
| `GET` | `/api/documentos/transportista/{id}/paginado` | Versión paginada. |
| `GET` | `/api/documentos/transportista/{id}/activos` | Documentos activos del transportista. |
| `GET` | `/api/documentos/me` | Documentos del transportista autenticado. |
| `PUT` | `/api/documentos/{id}` | Actualizar documento. |
| `PATCH` | `/api/documentos/{id}/estado` | Cambiar estado del documento. |
| `DELETE` | `/api/documentos/{id}` | Borrado lógico. |
| `PATCH` | `/api/documentos/{id}/restaurar` | Restaurar documento. |
| `DELETE` | `/api/documentos/{id}/permanente` | Borrado físico. |

## Auditoría (solo lectura) — `/api/auditoria/**`

Cada dominio principal expone un controlador de auditoría equivalente, con el mismo patrón de
endpoints:

| Método | Ruta (patrón) | Descripción |
|---|---|---|
| `GET` | `/api/auditoria/{dominio}` | Listar todos los registros de auditoría del dominio. |
| `GET` | `/api/auditoria/{dominio}/{entidad}/{id}` | Registros asociados a una entidad específica. |
| `GET` | `/api/auditoria/{dominio}/accion/{accion}` | Filtrar por tipo de acción. |
| `GET` | `/api/auditoria/{dominio}/fechas` | Filtrar por rango de fechas. |
| `GET` | `/api/auditoria/{dominio}/resumen` | Resumen agregado de auditoría. |

Dominios disponibles: `usuarios`, `pedidos`, `cargas`, `transportistas`, `documentos`.
`AuditoriaCargaController` y `AuditoriaDocumentoController` exponen además un filtro por
`transportista/{transportistaId}`.
