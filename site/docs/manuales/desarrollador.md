# Manual del desarrollador

Esta guía resume cómo extender cada componente del ecosistema AstraLog III, siguiendo los patrones ya establecidos en el código.

## Backend: cómo agregar un nuevo endpoint/dominio

Tomando como referencia el dominio `Pedido` (uno de los más completos), para agregar un nuevo recurso de negocio:

1. **Entidad** (`model/`): crear la clase extendiendo `BaseEntity`, con `@Entity`, `@Table` y Lombok (`@Getter @Setter @NoArgsConstructor @AllArgsConstructor @Builder`).
2. **Enums** (`model/enums/`, si aplica): definir los valores cerrados de dominio.
3. **Repository** (`repository/`): interfaz `extends JpaRepository<Entidad, Long>`, agregando métodos derivados (`findByX`, `existsByX`) según se necesiten.
4. **DTOs** (`dto/`): un `XxxRequestDTO` (con anotaciones Bean Validation: `@NotNull`, `@NotBlank`, `@DecimalMin`, etc. — **recomendado**, aunque no todos los DTOs existentes las usan de forma consistente, ver [Discrepancias](../anexos/discrepancias.md)) y, si la respuesta no debe exponer la entidad completa, un `XxxResponseDTO` con `@Builder`.
5. **Service** (`service/`): inyectar dependencias por constructor (`@RequiredArgsConstructor` + campos `final`), implementar la lógica de negocio, y llamar a `AuditService.auditXxx(...)` en cada operación de escritura si el dominio requiere trazabilidad.
6. **Controller** (`controller/`): `@RestController`, `@RequestMapping("/api/xxx")`, anotar cada endpoint con `@Operation`/`@ApiResponses` de Swagger para que aparezca correctamente en `/swagger-ui.html`.
7. **Seguridad**: si el endpoint requiere una regla de autorización distinta a "estar autenticado", agregarla explícitamente en `SecurityConfig.filterChain(...)` (vía `requestMatchers`) — **no asumir** que las descripciones de Swagger ("solo ADMIN") se traducen automáticamente en restricciones reales; deben aplicarse a nivel de Spring Security o con `@PreAuthorize`.
8. **Pruebas**: agregar pruebas unitarias (`*UnitTest`, ejecutadas por Surefire) y, si aplica, de integración (`*IntegrationTest`, ejecutadas por Failsafe con Testcontainers/H2).

### Buenas prácticas observadas en el código existente (a mantener)

- Soft delete vía `BaseEntity.softDelete(username)` en lugar de borrado físico, para entidades de negocio relevantes.
- Auditoría explícita desde el `Service`, nunca desde el `Controller`.
- DTOs de respuesta (`@Builder`) para no exponer relaciones JPA completas cuando no es necesario (aplicado de forma inconsistente actualmente — ver discrepancias).

### Mejoras recomendadas detectadas durante el análisis

- Introducir una capa `mapper` explícita (manual o con MapStruct) para reducir la lógica de mapeo embebida en los `Service`.
- Revisar el orden de los `requestMatchers` en `SecurityConfig` para evitar que reglas genéricas `permitAll()` puedan anular reglas más específicas de rol.
- Unificar el tipo de excepción usado para "recurso no encontrado" (hoy mezcla `RuntimeException` genérica, mapeada a `400`, con `EntityNotFoundException`, mapeada correctamente a `404`).
- Completar la cobertura de Bean Validation en todos los `RequestDTO` (actualmente `PedidoRequestDTO` y `DocumentoPersonalRequestDTO` no la usan).

## Frontend Angular: cómo agregar una nueva página

1. Crear una carpeta en `src/app/pages/<nombre>/` con `*.component.ts`, `*.component.html` y `*.component.css`, usando `standalone: true` (no se usan `NgModule` en este proyecto).
2. Si la página consume datos, crear o reutilizar un servicio en `src/app/service/`, siguiendo el patrón: `private readonly api = \`${environment.apiUrl}/recurso\`;` + métodos que devuelven `Observable<T>`.
3. Agregar el modelo TypeScript correspondiente en `src/app/models/` si no existe.
4. Registrar la ruta en `app.routes.ts`; **si la página debe estar protegida**, agregar `canActivate: [authGuard]` explícitamente (no es automático — ver [Discrepancias](../anexos/discrepancias.md) sobre `/documentos` y `/cargas`).
5. Si la página debe aparecer en el menú principal, agregar el enlace en `app.html` (topbar).
6. El interceptor `jwtInterceptor` ya está registrado globalmente en `app.config.ts`; no es necesario añadir el header `Authorization` manualmente en los servicios.

## App móvil Android: cómo agregar una nueva pantalla

1. Crear una carpeta en `ui/screens/<nombre>/` con `XxxScreen.kt` (Composable), `XxxViewModel.kt` (`AndroidViewModel` + `MutableStateFlow<XxxUiState>`) y `XxxUiState.kt` (data class de estado).
2. Si la pantalla consume datos nuevos del backend:
      - Agregar el método correspondiente a `ApiService.kt` (Retrofit), incluyendo `@Header("Authorization") token: String` si requiere autenticación.
      - Agregar DTOs de request/response en `data/remote/dto/` y `data/remote/response/` si no existen.
      - Crear o extender un `Repository` en `data/repository/`, envolviendo la llamada en `try/catch` y devolviendo `Resource<T>`.
3. En el `ViewModel`, obtener el token con `TokenManager(application).getToken()`, validarlo, y lanzar la llamada dentro de `viewModelScope.launch { ... }`, actualizando el `StateFlow` según el resultado (`Resource.Success`/`Resource.Error`).
4. Registrar la nueva pantalla como `composable("ruta") { ... }` dentro de `AppNavigation.kt`, conectando los callbacks de navegación (`onBack`, etc.) necesarios.

## Convenciones generales del proyecto

- **Idioma del código y los mensajes de negocio:** español (nombres de clases, mensajes de error, comentarios).
- **Gestión de errores HTTP del lado del backend:** centralizada en `GlobalExceptionHandler`; evitar capturar excepciones genéricas dentro de los controladores salvo casos justificados (como `AuthController`, que sí tiene manejo propio por la naturaleza de la autenticación).
- **Nomenclatura de auditoría:** acción siempre en mayúsculas (`CREATE`, `UPDATE`, `DELETE`).
- **No se usa Docker** en ningún componente actualmente; el flujo de desarrollo y despliegue documentado parte de ejecución directa (JVM, Node/Angular CLI, Gradle/Android Studio) — ver [Instalación](../instalacion.md).
