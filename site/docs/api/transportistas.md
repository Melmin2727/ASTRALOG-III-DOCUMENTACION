# API — Transportistas

Base path: `/api/transportistas`

## `POST /api/transportistas`

Crea un nuevo transportista. **Crea automáticamente su `Usuario` asociado** (ver [Backend → Modelo de dominio](../backend/modelo-dominio.md)).

**Autenticación requerida:** JWT válido.

### Request Body (`TransportistaRequestDTO`)

```json
{
  "nombre": "Juan",
  "apellidos": "Pérez Quispe",
  "dni": "12345678",
  "edad": 28,
  "tipoTransporte": "CAMIONERO",
  "placa": "ABC-123",
  "vehiculoInfo": "Volvo FH 2019",
  "capacidad": 12.5,
  "estado": "ACTIVO",
  "usuarioId": 0
}
```

| Campo | Tipo | Validación |
|---|---|---|
| `nombre` | string | `@NotBlank` |
| `apellidos` | string | `@NotBlank` |
| `dni` | string | `@NotBlank`, longitud exacta 8 (`@Size(min=8, max=8)`) |
| `edad` | int | `@Min(18)` |
| `tipoTransporte` | string (`VOLQUETERO`\|`CAMIONERO`) | `@NotNull` |
| `placa` | string | `@NotBlank` |
| `vehiculoInfo` | string | Opcional |
| `capacidad` | double | `@NotNull`, `@Positive` |
| `estado` | string | Opcional; por defecto `ACTIVO` si se omite |
| `usuarioId` | long | `@NotNull` en el DTO, aunque **no se utiliza** en `TransportistaService.crear(...)`: el usuario se genera automáticamente y no se vincula al `usuarioId` recibido — ver discrepancia abajo. |

!!! warning "El campo `usuarioId` del DTO no se usa realmente"
    `TransportistaRequestDTO.usuarioId` está marcado `@NotNull`, pero `TransportistaService.crear(...)` **ignora ese valor por completo**: siempre crea un `Usuario` nuevo con username/password autogenerados. Esto puede confundir a los consumidores de la API, que deben enviar igualmente un valor numérico (por validación) aunque no tenga efecto. Ver [Discrepancias](../anexos/discrepancias.md).

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Transportista registrado; devuelve la entidad `Transportista` completa (incluye el `Usuario` anidado). |
| `400 Bad Request` | Datos inválidos. |

---

## `GET /api/transportistas`

Lista todos los transportistas.

**Autenticación requerida:** JWT válido (documentado como "solo ADMIN" en Swagger, sin aplicación técnica real de esa restricción — ver [Seguridad](../backend/seguridad.md)).

### Respuesta `200 OK`

```json
[
  {
    "id": 1,
    "usuario": { "id": 5, "username": "juan.perez", "rol": "TRANSPORTISTA", "activo": true },
    "nombre": "Juan",
    "apellidos": "Pérez Quispe",
    "dni": "12345678",
    "edad": 28,
    "tipoTransporte": "CAMIONERO",
    "placa": "ABC-123",
    "vehiculoInfo": "Volvo FH 2019",
    "capacidad": 12.5,
    "documentos": [],
    "estado": "ACTIVO"
  }
]
```

---

## `GET /api/transportistas/tipo/{tipo}`

Filtra transportistas **activos** por tipo de transporte.

### Path parameters

| Parámetro | Tipo | Valores |
|---|---|---|
| `tipo` | enum | `VOLQUETERO`, `CAMIONERO` |

---

## `GET /api/transportistas/{id}/documentos`

Obtiene los documentos personales de un transportista.

### Path parameters

| Parámetro | Tipo |
|---|---|
| `id` | long |

---

## `GET /api/transportistas/me`

Obtiene el perfil del transportista autenticado (resuelve el `Usuario` desde el JWT y busca su `Transportista` asociado).

**Autenticación requerida:** JWT válido.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Perfil del transportista autenticado. |
| `400 Bad Request` | Si no hay autenticación, o si el usuario/transportista no existe (vía `RuntimeException`/`IllegalArgumentException`, mapeados a `400`). |

---

## `PUT /api/transportistas/{id}`

Actualiza los datos de un transportista existente (mismo esquema que `POST`).

---

## `DELETE /api/transportistas/{id}`

Elimina lógicamente un transportista: aplica `softDelete` **y** cambia explícitamente su `estado` a `INACTIVO`.

**Autenticación requerida:** JWT válido. A diferencia de `UsuarioController`, aquí **sí** se usa el username del usuario autenticado real (`authentication.getName()`) como responsable del borrado, con fallback a `"SYSTEM"` si no hay autenticación.
