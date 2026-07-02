# Discrepancias detectadas

Esta página consolida todas las diferencias encontradas entre (a) lo solicitado/asumido en el prompt de referencia del proyecto y (b) el comportamiento real del código fuente analizado el 2026-06-30, así como inconsistencias internas detectadas entre componentes. En todos los casos, **esta documentación prioriza el código fuente** como fuente de verdad.

## 1. Repositorios oficiales de AstraLog III (Ramas de entrega final)

| # | Repositorio | Rama | Enlace |
|---|---|---|---|
| 1.1 | Página Publicitaria - AstraLog III | main | https://github.com/elvisjsAtlas1/PaginaPublicitaria-AstramacoIII |
| 1.2 | Frontend-AstramacoIII | entrega-final | https://github.com/elvisjsAtlas1/Frontend-AstramacoIII/tree/entrega-final |
| 1.3 | Backend-AstramacoIII | final-trabajo | https://github.com/elvisjsAtlas1/Backend-AstramacoIII/tree/final-trabajo |
| 1.4 | AppAstraLog-AstramacoIII | entrega-final | https://github.com/elvisjsAtlas1/AppAstraLog-AstramacoIII/tree/entrega-final |

## 2. Arquitectura y módulos

| # | Discrepancia | Detalle |
|---|---|---|
| 2.1 | Sin capa `mapper` dedicada en el backend | El prompt asumía una arquitectura con paquete `mapper`/Mappers explícitos. El mapeo Entity↔DTO se realiza manualmente dentro de cada `Service`. |
| 2.2 | La página publicitaria no está integrada con el backend | Es un sitio estático sin llamadas HTTP a la API ni formularios conectados a un servidor. |
| 2.3 | Sin infraestructura Docker | No se encontró `Dockerfile` ni `docker-compose.yml` en ningún repositorio de AstraLog III, pese a que el prompt solicitaba documentarlo "si existe". |

## 3. Backend — Seguridad

| # | Discrepancia | Detalle |
|---|---|---|
| 3.1 | Regla de autorización contradictoria en `/api/usuarios` | `/api/usuarios` aparece simultáneamente en una lista `permitAll()` genérica y en una regla específica `hasRole("ADMIN")` para `POST`. Spring Security evalúa las reglas en el orden declarado, por lo que la regla `permitAll()` puede aplicarse antes que la restricción de rol pretendida. |
| 3.2 | La mayoría de endpoints no aplican control de rol real | Las descripciones Swagger de varios endpoints (p. ej. "solo ADMIN puede acceder") son anotaciones documentales, no restricciones técnicas: no se encontró ningún uso de `@PreAuthorize`/`@Secured`/`hasRole` salvo en `POST /api/usuarios`. |
| 3.3 | Doble configuración CORS | `SecurityConfig.corsConfigurationSource()` y `CorsConfig.corsConfigurer()` definen configuraciones CORS independientes y redundantes (aunque consistentes entre sí). |
| 3.4 | `AuditService.auditAuth(...)` nunca se invoca | El método está completamente implementado, pero `AuthController` no lo llama; los intentos de login (éxito o fallo) no quedan auditados pese a que la infraestructura existe. |
| 3.5 | Contraseña inicial del transportista = su DNI | Al crear un transportista, su `Usuario` se genera con password inicial igual a su DNI (encriptado). No existe una funcionalidad de cambio de contraseña obligatorio en el primer login. |

## 4. Backend — Manejo de errores

| # | Discrepancia | Detalle |
|---|---|---|
| 4.1 | "No encontrado" responde mayormente `400`, no `404` | La mayoría de los servicios lanzan `RuntimeException`, que `GlobalExceptionHandler` traduce a `400 Bad Request`. Solo `DocumentoPersonalService` usa `EntityNotFoundException`, que sí produce `404`. La documentación Swagger de varios endpoints declara `404` como posible respuesta, pero en la práctica no ocurre así (salvo para documentos y para `DELETE /api/usuarios/{id}`, manejado explícitamente). |

## 5. Backend — Endpoints faltantes o no usados

| # | Discrepancia | Detalle |
|---|---|---|
| 5.1 | Sin endpoints de auditoría para pedidos, cargas y documentos | `AuditService` registra estos eventos en base de datos, pero no existen controladores que los expongan (a diferencia de usuarios y transportistas). |
| 5.2 | `CargaService.eliminarCarga(...)` sin endpoint | El método existe y registra auditoría, pero `CargaController` no expone ningún `DELETE`. |
| 5.3 | `DocumentoPersonalService.actualizar(...)` y `.eliminar(...)` sin endpoint | `DocumentoPersonalController` solo expone `POST` y `GET`; no hay forma de actualizar o eliminar un documento vía API pública. |
| 5.4 | Validación inconsistente entre DTOs | `TransportistaRequestDTO` y los DTOs de `Carga` usan Bean Validation (`@NotNull`, `@NotBlank`, etc.); `PedidoRequestDTO` y `DocumentoPersonalRequestDTO` no declaran ninguna anotación de validación, dependiendo enteramente de validación imperativa en el `Service`. |
| 5.5 | `TransportistaRequestDTO.usuarioId` no se usa | El campo es `@NotNull` en el DTO, pero `TransportistaService.crear(...)` lo ignora por completo: siempre genera un `Usuario` nuevo. |
| 5.6 | `POST /api/usuarios` expone el hash de la contraseña | El endpoint retorna la entidad `Usuario` completa (incluyendo `password` con el hash BCrypt) en lugar de un DTO de respuesta sin ese campo. |
| 5.7 | `username` de auditoría hardcodeado en `UsuarioController.eliminar` | Se registra como responsable del borrado el valor fijo `"usuario_sistema"`, en lugar del usuario autenticado real (a diferencia de `TransportistaController.eliminar`, que sí usa `authentication.getName()`). |
| 5.8 | Código de verificación de pedidos fijo | Todos los pedidos se crean con `codigoVerificacion = "1234"` (constante), no un código generado dinámicamente por pedido. |

## 6. Frontend web

| # | Discrepancia | Detalle |
|---|---|---|
| 6.1 | Rutas `/documentos` y `/cargas` sin `authGuard` | A diferencia de `/pedidos` y `/transportistas`, estas rutas no tienen `canActivate: [authGuard]` en `app.routes.ts`. |
| 6.2 | `authGuard` no valida la validez real del JWT | Solo comprueba que exista *algún* valor en `localStorage`, sin verificar firma ni expiración. |
| 6.3 | `LoginComponent` no maneja errores de login | El `subscribe(...)` del método `login()` no define un callback `error`, por lo que un login fallido no muestra ninguna retroalimentación visible al usuario. |
| 6.4 | El frontend web no permite crear/actualizar cargas | `CargaService` (frontend) solo implementa lecturas; la creación/aumento de inventario solo es posible desde la app móvil o directamente contra la API. |
| 6.5 | Inconsistencia de estados de pedido entre frontend y backend | El modelo `Pedido` del frontend incluye el estado `'PENDIENTE'`, que no existe en el enum `EstadoPedido` real del backend (`EN_ENVIO`, `EN_DESCARGA`, `ENTREGADO`, `CANCELADO`). |

## 7. Aplicación móvil

| # | Discrepancia | Detalle |
|---|---|---|
| 7.1 | URL del backend hardcodeada en el código fuente | `Constants.BASE_URL` apunta a una IP de red local fija (`192.168.0.102`), sin mecanismo de configuración por entorno/build variant; debe editarse y recompilarse manualmente para cada red. |
| 7.2 | Cobertura funcional parcial respecto al backend | La app no implementa edición de perfil, gestión de documentos personales, ni creación/edición de pedidos — solo consulta y, para `CAMIONERO`, gestión de carga. |

## Resumen por severidad

| Severidad | Cantidad | Ítems |
|---|---|---|
| 🔴 Alta (seguridad / integridad de datos) | 3 | 3.1, 3.5, 5.6 |
| 🟠 Media (funcionalidad incompleta o inconsistente) | 9 | 1.1, 4.1, 5.1, 5.2, 5.3, 5.5, 6.1, 6.3, 6.5 |
| 🟡 Baja (mejoras de mantenibilidad/configuración) | 9 | 2.1, 2.3, 3.2, 3.3, 3.4, 5.4, 5.7, 5.8, 6.2, 6.4, 7.1, 7.2 |

!!! info "Sobre el uso de esta lista"
    Estas observaciones reflejan el estado del código en el momento del análisis (commits más recientes de cada rama `main`/por defecto al 2026-06-30) y están pensadas como insumo para el backlog técnico del equipo, no como una crítica al proyecto en su conjunto. Varias de ellas (5.4, 5.5, 6.3) son típicas de un sistema en desarrollo activo y de bajo riesgo si se abordan antes de un despliegue productivo.
