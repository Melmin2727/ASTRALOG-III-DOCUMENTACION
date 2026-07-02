# API — Autenticación

Base path: `/api/auth`

## `POST /api/auth/login`

Autentica a un usuario y retorna un token JWT.

**Autenticación requerida:** No (endpoint público).

### Request Body

```json
{
  "username": "admin",
  "password": "********"
}
```

| Campo | Tipo | Obligatorio | Validación |
|---|---|---|---|
| `username` | string | Sí | `@NotBlank` — "El username es obligatorio" |
| `password` | string | Sí | `@NotBlank` — "La contraseña es obligatoria" |

### Respuestas

=== "200 OK"

    Login exitoso, token generado.

    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiJ9...",
      "username": "admin",
      "rol": "ADMIN"
    }
    ```

=== "400 Bad Request"

    Falla de validación (campo vacío).

    ```json
    {
      "timestamp": "2026-06-30T10:00:00",
      "message": "username: El username es obligatorio",
      "status": 400,
      "error": "Bad Request"
    }
    ```

=== "401 Unauthorized"

    Credenciales inválidas, usuario deshabilitado o bloqueado.

    ```json
    { "error": "Credenciales inválidas" }
    ```

=== "500 Internal Server Error"

    Error inesperado durante la autenticación.

    ```json
    { "error": "Error interno del servidor" }
    ```

### Notas de implementación

- El token generado debe enviarse en las siguientes peticiones como header `Authorization: Bearer {token}`.
- El intento de login **no queda registrado en la tabla de auditoría** (`AuditService.auditAuth` existe pero no se invoca desde este controlador) — ver [Discrepancias](../anexos/discrepancias.md).
- El campo `rol` del response corresponde al valor del enum `Rol` (`ADMIN`, `USER`, `TRANSPORTISTA`) del usuario autenticado.
