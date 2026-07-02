# Manejo de errores

El backend centraliza el manejo de excepciones con `@RestControllerAdvice` en `exception.GlobalExceptionHandler`, evitando lógica de manejo de errores dispersa en los controladores.

## Excepciones manejadas

| Excepción | Código HTTP | Cuerpo de respuesta | Cuándo ocurre |
|---|---|---|---|
| `MethodArgumentNotValidException` | `400 Bad Request` | `{ timestamp, message, status, error: "Bad Request" }` — `message` toma el **primer** error de validación encontrado (`campo: mensaje`). | Falla una anotación Bean Validation (`@NotNull`, `@NotBlank`, `@DecimalMin`, etc.) sobre un DTO con `@Valid`. |
| `DataIntegrityViolationException` | `400 Bad Request` | `{ error: "..." }` — mensaje especial si detecta `"Duplicate entry"` en la causa. | Violaciones de restricciones de base de datos (p. ej. `username` duplicado). |
| `IllegalArgumentException` | `400 Bad Request` | `{ timestamp, message, status, error: "Bad Request" }` | Reglas de negocio violadas (tipo de transporte incorrecto, material no disponible, stock insuficiente, etc.). |
| `RuntimeException` (genérica) | `400 Bad Request` | `{ timestamp, message, status, error: "Bad Request" }` | Captura general para errores de negocio no tipados explícitamente (p. ej. `"Transportista no encontrado"`). |
| `EntityNotFoundException` (Jakarta Persistence) | `404 Not Found` | `{ timestamp, message, status, error: "Not Found" }` | Entidad no encontrada en `DocumentoPersonalService` (uso explícito de esta excepción en lugar de `RuntimeException`). |

!!! note "Inconsistencia en el modelado de \"no encontrado\""
    La mayoría de los servicios (`PedidoService`, `CargaService`, `TransportistaService`, `UsuarioService`) lanzan `RuntimeException("X no encontrado")` para casos de "recurso no encontrado", lo cual el `GlobalExceptionHandler` traduce a **`400 Bad Request`** en lugar de `404 Not Found`. Solo `DocumentoPersonalService` usa la excepción semánticamente correcta `EntityNotFoundException`, que sí produce `404`. Esto significa que, salvo para documentos, **un "recurso no encontrado" responde con 400 en vez de 404** en la mayor parte de la API. Ver [Discrepancias](../anexos/discrepancias.md).

## Ejemplo de respuesta de error de validación

```json
{
  "timestamp": "2026-06-30T10:15:30",
  "message": "cantidadDisponible: La cantidad no puede ser negativa",
  "status": 400,
  "error": "Bad Request"
}
```

## Ejemplo de respuesta de violación de integridad

```json
{
  "error": "El registro o nombre de usuario ya existe en el sistema."
}
```

## Manejo de errores en `AuthController` (caso especial)

`AuthController.login(...)` no delega en `GlobalExceptionHandler` para sus propios errores: captura explícitamente `BadCredentialsException`, `DisabledException`, `LockedException`, `AuthenticationException` genérica y `Exception` genérica, devolviendo en todos los casos de credenciales inválidas `401 Unauthorized` con `{ "error": "Credenciales inválidas" }` (o `"Usuario deshabilitado"` / `"Usuario bloqueado"` según el caso), y `500 Internal Server Error` ante errores inesperados.
