# API — Usuarios

Base path: `/api/usuarios`

## `POST /api/usuarios`

Registra un nuevo usuario con un rol específico.

**Autenticación requerida:** Declarada como `hasRole("ADMIN")` en `SecurityConfig` (ver advertencia de seguridad en [Backend → Seguridad](../backend/seguridad.md)).

### Request Body (`UsuarioRequestDTO`)

```json
{
  "username": "nuevo.usuario",
  "password": "claveSegura123",
  "rol": "ADMIN"
}
```

| Campo | Tipo | Notas |
|---|---|---|
| `username` | string | Debe ser único; si se repite, responde `400` con mensaje de duplicado. |
| `password` | string | Se encripta con BCrypt antes de persistir. |
| `rol` | string (`ADMIN`\|`USER`\|`TRANSPORTISTA`) | Enum `Rol`. |

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Usuario creado; devuelve la entidad `Usuario` completa (incluye `password` hasheada — ver nota). |
| `400 Bad Request` | Datos inválidos o `username` duplicado. |

!!! warning "El endpoint devuelve la entidad `Usuario` completa, incluyendo el hash de la contraseña"
    `POST /api/usuarios` retorna directamente el objeto `Usuario` (no un DTO de respuesta sin contraseña), por lo que el hash BCrypt de la contraseña viaja en la respuesta JSON. Aunque no es la contraseña en texto plano, exponer el hash no es una práctica recomendada. Ver [Discrepancias](../anexos/discrepancias.md).

---

## `PUT /api/usuarios/{id}`

Actualiza los datos de un usuario existente.

**Autenticación requerida:** JWT válido (no hay restricción de rol explícita — ver [Seguridad](../backend/seguridad.md)).

### Path parameters

| Parámetro | Tipo | Descripción |
|---|---|---|
| `id` | long | ID del usuario a actualizar. |

### Request Body (`UsuarioRequestDTO`)

Mismo esquema que en la creación. Nota: el controlador fija `activo = true` de forma fija al actualizar, independientemente del estado previo.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Usuario actualizado, devuelve la entidad `Usuario`. |
| `404 Not Found` | (Documentado en Swagger) — en la práctica, según `GlobalExceptionHandler`, un `RuntimeException("Usuario no encontrado")` se traduce a **`400 Bad Request`**, no `404`. Ver [Discrepancias](../anexos/discrepancias.md). |

---

## `DELETE /api/usuarios/{id}`

Elimina (soft delete) un usuario por su ID.

**Autenticación requerida:** JWT válido.

### Path parameters

| Parámetro | Tipo | Descripción |
|---|---|---|
| `id` | long | ID del usuario a eliminar. |

### Respuestas

| Código | Descripción |
|---|---|
| `204 No Content` | Usuario eliminado (soft delete) correctamente. |
| `404 Not Found` | El controlador sí maneja explícitamente este caso (`if e.getMessage().equals("Usuario no encontrado")`) y retorna `404` manualmente — este es el único caso de "no encontrado" para usuarios que efectivamente produce un `404` real. |

### Notas de implementación

- La eliminación es **lógica** (`softDelete`): el registro permanece en base de datos con `deletedAt`/`deletedBy` poblados, y queda excluido de las consultas gracias al filtro `@Where(clause = "deleted_at IS NULL")` en la entidad `Usuario`.
- El parámetro `username` registrado como responsable del borrado está hardcodeado como `"usuario_sistema"` en `UsuarioController.eliminar(...)`, en lugar de tomar el usuario autenticado real.
