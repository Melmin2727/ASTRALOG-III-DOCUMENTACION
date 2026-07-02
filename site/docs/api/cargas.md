# API — Cargas

Base path: `/api/cargas`

La gestión de cargas (inventario de materiales) está restringida por reglas de negocio a transportistas de tipo **`CAMIONERO`**, que solo pueden manejar los materiales **`PANDERETA`** o **`TECHO`**.

## `PUT /api/cargas/{transportistaId}`

Actualiza o asigna (crea si no existe) la carga actual de un transportista.

**Autenticación requerida:** JWT válido.

### Path parameters

| Parámetro | Tipo | Ejemplo |
|---|---|---|
| `transportistaId` | long | `1` |

### Request Body (`CargaRequestDTO`)

```json
{
  "tipoMaterial": "PANDERETA",
  "cantidadDisponible": 1000
}
```

| Campo | Validación |
|---|---|
| `tipoMaterial` | `@NotNull` — debe ser `PANDERETA` o `TECHO`, validado además en el servicio. |
| `cantidadDisponible` | `@NotNull`, `@DecimalMin(0.0, inclusive=true)` |

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Carga actualizada/creada, devuelve `CargaResponseDTO`. |
| `400 Bad Request` | Transportista no es `CAMIONERO`, material inválido o datos inválidos. |
| `404 Not Found` | (Documentado en Swagger) — el servicio en realidad lanza `RuntimeException`, mapeada por `GlobalExceptionHandler` a `400`. |

---

## `POST /api/cargas/{transportistaId}/aumentar`

Incrementa la cantidad disponible de una carga **ya existente** del mismo material.

### Request Body (`AumentarCargaRequestDTO`)

```json
{
  "tipoMaterial": "PANDERETA",
  "cantidadAgregar": 200
}
```

| Campo | Validación |
|---|---|
| `tipoMaterial` | `@NotNull` |
| `cantidadAgregar` | `@NotNull`, `@DecimalMin(0.0, inclusive=false)` (debe ser mayor a cero) |

!!! note "El material debe coincidir con la carga existente"
    Si `tipoMaterial` no coincide con el material ya registrado en la `Carga` del transportista, el servicio rechaza la operación con `400` ("Solo se puede aumentar si el material es el mismo que la carga actual"). Para registrar un material distinto, debe usarse `PUT /api/cargas/{transportistaId}`, que sobrescribe la carga.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Carga aumentada. |
| `400 Bad Request` | Cantidad inválida, sin inventario previo, o material distinto al ya registrado. |

---

## `GET /api/cargas/{transportistaId}`

Obtiene la carga actual de un transportista específico.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | `CargaResponseDTO` con la carga actual. |
| `404 Not Found` | (Documentado en Swagger) — mapeado realmente a `400` por `GlobalExceptionHandler`. |

---

## `GET /api/cargas`

Lista todas las cargas registradas en el sistema (documentado en Swagger como "solo ADMIN", sin restricción técnica real — ver [Seguridad](../backend/seguridad.md)).

### Respuesta `200 OK`

```json
[
  {
    "id": 3,
    "transportistaId": 1,
    "transportistaNombre": "Juan Pérez Quispe",
    "tipoMaterial": "PANDERETA",
    "cantidadDisponible": 800
  }
]
```

!!! info "Sin endpoint `DELETE` expuesto"
    `CargaService.eliminarCarga(...)` existe en el código y registra correctamente la auditoría de eliminación, pero **no hay ningún endpoint en `CargaController` que lo invoque**. Esta operación solo es alcanzable internamente, no desde la API pública. Ver [Discrepancias](../anexos/discrepancias.md).
