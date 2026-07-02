# API REST — Introducción

Esta sección documenta, endpoint por endpoint, la API REST real expuesta por `Backend-AstramacoIII`, extraída directamente de las anotaciones `@RestController`/`@RequestMapping`/`@GetMapping`/etc. y de los DTOs de cada controlador.

- **URL base (desarrollo):** `http://localhost:8080/api`
- **Formato:** JSON
- **Autenticación:** JWT vía header `Authorization: Bearer {token}` (excepto `/api/auth/login` y la documentación Swagger/OpenAPI).
- **Documentación interactiva autogenerada:** `http://localhost:8080/swagger-ui.html` (springdoc-openapi 2.5.0).

!!! tip "Fuente de la verdad"
    La referencia más actualizada y ejecutable de la API siempre es el propio `/swagger-ui.html` del backend en ejecución. Esta documentación describe el comportamiento tal como está implementado en el código fuente analizado.

## Convención de errores

Todas las respuestas de error (salvo las de `/api/auth/login`, que tienen su propio formato) siguen el formato producido por `GlobalExceptionHandler` — ver [Backend → Manejo de errores](../backend/manejo-errores.md):

```json
{
  "timestamp": "2026-06-30T10:15:30",
  "message": "Descripción del error",
  "status": 400,
  "error": "Bad Request"
}
```

## Índice de recursos

| Recurso | Base path | Autenticación |
|---|---|---|
| [Autenticación](autenticacion.md) | `/api/auth` | Pública |
| [Usuarios](usuarios.md) | `/api/usuarios` | `POST` requiere rol `ADMIN`*; resto requiere JWT |
| [Transportistas](transportistas.md) | `/api/transportistas` | Requiere JWT |
| [Pedidos](pedidos.md) | `/api/pedidos` | Requiere JWT |
| [Cargas](cargas.md) | `/api/cargas` | Requiere JWT |
| [Documentos personales](documentos.md) | `/api/documentos` | Requiere JWT |
| [Auditoría](auditoria.md) | `/api/auditoria` | Requiere JWT |

\* Ver advertencia de seguridad en [Backend → Seguridad y JWT](../backend/seguridad.md) sobre el conflicto entre reglas `permitAll()` y `hasRole("ADMIN")` para `/api/usuarios`.
