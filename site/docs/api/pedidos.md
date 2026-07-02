# API — Pedidos

Base path: `/api/pedidos`

## `POST /api/pedidos`

Registra un nuevo pedido de transporte de materiales.

**Autenticación requerida:** JWT válido.

### Request Body (`PedidoRequestDTO`)

```json
{
  "clienteNombre": "María López",
  "clienteTelefono": "987654321",
  "direccionEnvio": "Jr. Lima 123, Juliaca",
  "tipoTransporte": "CAMIONERO",
  "material": "PANDERETA",
  "cantidad": 500,
  "montoTotal": 1200.0,
  "adelanto": 200.0,
  "piso": 2,
  "horaEnvio": "2026-07-01T09:00:00",
  "transportistaId": 1
}
```

| Campo | Tipo | Notas |
|---|---|---|
| `clienteNombre` | string | Sin validación `@NotBlank` declarada en el DTO. |
| `clienteTelefono` | string | Sin validación declarada. |
| `direccionEnvio` | string | Sin validación declarada. |
| `tipoTransporte` | enum (`VOLQUETERO`\|`CAMIONERO`) | Debe coincidir con el tipo real del `Transportista` indicado, o falla con `400`. |
| `material` | enum `TipoMaterial` | Si `tipoTransporte = CAMIONERO`, solo se acepta `PANDERETA` o `TECHO`. |
| `cantidad` | double | Se descuenta del stock de `Carga` si el transportista es `CAMIONERO`. |
| `montoTotal` / `adelanto` | double | Sin validación declarada. |
| `piso` | int | Sin validación declarada. |
| `horaEnvio` | datetime ISO-8601 | — |
| `transportistaId` | long | Debe existir, o responde `400` ("Transportista no encontrado"). |

!!! note "DTO de pedido sin anotaciones de validación"
    A diferencia de `CargaRequestDTO`/`TransportistaRequestDTO`, `PedidoRequestDTO` **no declara ninguna anotación Bean Validation** (`@NotBlank`, `@NotNull`, etc.) y el controlador no usa `@Valid`. Las únicas validaciones aplicadas ocurren manualmente dentro de `PedidoService` (tipo de transporte, material, stock). Campos como `clienteNombre` vacío o `cantidad` negativa **no son rechazados explícitamente** por una validación declarativa. Ver [Discrepancias](../anexos/discrepancias.md).

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Pedido creado; devuelve `PedidoResponseDTO` con `estado: "EN_ENVIO"` y `codigoVerificacion: "1234"` (valor fijo, ver [Modelo de dominio](../backend/modelo-dominio.md)). |
| `400 Bad Request` | Transportista no encontrado, tipo de transporte incorrecto, material no disponible o stock insuficiente. |

---

## `GET /api/pedidos`

Lista todos los pedidos del sistema.

**Autenticación requerida:** JWT válido (documentado en Swagger como "Solo ADMIN puede acceder", sin restricción técnica real de rol aplicada — ver [Seguridad](../backend/seguridad.md)).

### Respuesta `200 OK`

```json
[
  {
    "id": 10,
    "clienteNombre": "María López",
    "clienteTelefono": "987654321",
    "direccionEnvio": "Jr. Lima 123, Juliaca",
    "tipoTransporte": "CAMIONERO",
    "material": "PANDERETA",
    "cantidad": 500,
    "montoTotal": 1200.0,
    "adelanto": 200.0,
    "piso": 2,
    "horaEnvio": "2026-07-01T09:00:00",
    "transportistaId": 1,
    "transportistaNombre": "Juan Pérez Quispe",
    "estado": "EN_ENVIO",
    "codigoVerificacion": "1234"
  }
]
```

---

## `GET /api/pedidos/me`

Lista los pedidos del transportista autenticado.

**Autenticación requerida:** JWT válido. Resuelve el `Usuario` autenticado → su `Transportista` asociado → sus pedidos (ordenados por `horaEnvio` descendente).

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Lista de pedidos del transportista autenticado. |
| `404 Not Found` | (Documentado en Swagger) — en la práctica, según `GlobalExceptionHandler`, este caso se traduce a `400 Bad Request`, no `404`. |
