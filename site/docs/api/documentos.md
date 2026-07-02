# API — Documentos personales

Base path: `/api/documentos`

## `POST /api/documentos/{transportistaId}`

Registra un nuevo documento personal para un transportista específico.

**Autenticación requerida:** JWT válido.

### Path parameters

| Parámetro | Tipo |
|---|---|
| `transportistaId` | long |

### Request Body (`DocumentoPersonalRequestDTO`)

```json
{
  "tipoDocumento": "SOAT",
  "valor": "AB123456",
  "fechaEmision": "2026-01-15",
  "fechaVencimiento": "2027-01-15"
}
```

| Campo | Tipo | Reglas |
|---|---|---|
| `tipoDocumento` | enum `TipoDocumento` | `SOAT`, `REVISION_TECNICA`, `LICENCIA`, `TARJETA_CIRCULACION`, `DNI` |
| `valor` | string | Para `LICENCIA`/`TARJETA_CIRCULACION` debe ser literalmente `"SI"` o `"NO"`. |
| `fechaEmision` | date (`YYYY-MM-DD`) | **Obligatoria** si `tipoDocumento` es `SOAT` o `REVISION_TECNICA`; ignorada/forzada a `null` para los demás tipos. |
| `fechaVencimiento` | date (`YYYY-MM-DD`) | Misma regla que `fechaEmision`. |

!!! note "Sin Bean Validation declarativa en el DTO"
    `DocumentoPersonalRequestDTO` no usa anotaciones `@NotNull`/`@NotBlank`; toda la validación de reglas (fechas obligatorias, valores `SI`/`NO`) ocurre de forma imperativa en `DocumentoPersonalService.validarDocumento(...)` y en el `@PrePersist`/`@PreUpdate` de la entidad `DocumentoPersonal`.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Documento guardado; devuelve la entidad `DocumentoPersonal` completa. |
| `400 Bad Request` | Documento de ese tipo ya registrado para el transportista, o fechas/valores inválidos. |
| `404 Not Found` | (Documentado en Swagger) — el servicio usa `EntityNotFoundException`, que **sí** produce un `404` real (a diferencia de la mayoría de los demás dominios). |

!!! tip "Un solo documento por tipo y transportista"
    `DocumentoPersonalService.guardar(...)` verifica `existsByTransportistaIdAndTipoDocumento(...)` antes de insertar: un transportista no puede tener dos documentos del mismo `tipoDocumento` simultáneamente. Para "renovar" un documento (p. ej. un nuevo SOAT), debe usarse la actualización a nivel de servicio (`DocumentoPersonalService.actualizar`), aunque **no existe un endpoint `PUT` expuesto en `DocumentoPersonalController`** para esa operación — ver discrepancia abajo.

---

## `GET /api/documentos/transportista/{id}`

Obtiene todos los documentos de un transportista por su ID.

### Respuestas

| Código | Descripción |
|---|---|
| `200 OK` | Lista de `DocumentoPersonal` del transportista. |
| `404 Not Found` | Si el transportista no existe (documentado; el método de listado en sí no valida explícitamente la existencia del transportista, solo filtra por `transportistaId`, por lo que en la práctica devolvería una lista vacía en vez de un error). |

!!! warning "Endpoints de actualización y eliminación de documentos no expuestos"
    `DocumentoPersonalService` implementa `actualizar(...)` y `eliminar(...)` (con su correspondiente auditoría), pero `DocumentoPersonalController` **solo expone `POST` y `GET`**. No hay forma de actualizar o eliminar un documento personal a través de la API pública actual. Ver [Discrepancias](../anexos/discrepancias.md).
