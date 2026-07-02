# API — Auditoría

Endpoints de **solo lectura** sobre los registros históricos generados por `AuditService` — ver [Backend → Auditoría](../backend/auditoria.md).

## `GET /api/auditoria/transportistas`

Lista todo el histórico de auditoría de transportistas, ordenado por fecha descendente (`findAllByOrderByFechaHoraDesc`).

**Autenticación requerida:** JWT válido.

### Respuesta `200 OK`

```json
[
  {
    "id": 42,
    "transportistaId": 1,
    "accion": "UPDATE",
    "username": "admin",
    "nombreAnterior": "Juan",
    "nombreNuevo": "Juan Carlos",
    "estadoAnterior": "ACTIVO",
    "estadoNuevo": "ACTIVO",
    "fechaHora": "2026-06-29T18:42:11",
    "ipAddress": "192.168.0.10",
    "datosCompletosAnteriores": "{...}",
    "datosCompletosNuevos": "{...}"
  }
]
```

## `GET /api/auditoria/transportistas/transportista/{transportistaId}`

Histórico de auditoría de un transportista específico (`findByTransportistaIdOrderByFechaHoraDesc`).

---

## `GET /api/auditoria/usuarios`

Lista todo el histórico de auditoría de usuarios.

## `GET /api/auditoria/usuarios/usuario/{usuarioId}`

Histórico de auditoría de un usuario específico.

---

!!! warning "Sin endpoints de auditoría para pedidos, cargas y documentos"
    Como se detalla en [Backend → Auditoría](../backend/auditoria.md), no existen endpoints `GET /api/auditoria/pedidos`, `/cargas` ni `/documentos`, aunque sus eventos sí se registran en base de datos.

!!! warning "Sin restricción de rol explícita"
    Ninguno de los endpoints de auditoría exige un rol específico a nivel de Spring Security; cualquier usuario autenticado (independientemente de su rol) puede consultar el histórico completo de auditoría de transportistas y usuarios, incluyendo datos sensibles serializados en `datosCompletosAnteriores`/`datosCompletosNuevos`. Ver [Discrepancias](../anexos/discrepancias.md).
