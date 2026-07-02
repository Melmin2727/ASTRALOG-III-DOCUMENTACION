# Backend — Visión general

El backend de AstraLog III (`Backend-AstramacoIII`) es una API REST construida con **Spring Boot 3.3.5** sobre **Java 17**, que centraliza toda la lógica de negocio, persistencia, seguridad y auditoría del sistema.

- **Puerto por defecto:** `8080` (`server.port: 8080`, escuchando en `0.0.0.0`).
- **Base path de la API:** `/api`.
- **Documentación interactiva:** expuesta automáticamente vía springdoc-openapi en `/swagger-ui.html` y el contrato OpenAPI en `/v3/api-docs`.
- **Base de datos:** MySQL (`astramaco_db`), con esquema autogenerado por Hibernate (`ddl-auto: update`).

## Responsabilidades principales

1. **Gestión de usuarios** y autenticación basada en JWT.
2. **Gestión de transportistas** (camioneros y volqueteros), incluyendo la creación automática de su cuenta de usuario asociada.
3. **Gestión de documentos personales** de los transportistas (SOAT, revisión técnica, licencia, tarjeta de circulación, DNI).
4. **Gestión de cargas** (inventario de materiales que un transportista camionero tiene disponible).
5. **Gestión de pedidos**, con validación cruzada de tipo de transporte/material y descuento automático de stock de carga.
6. **Auditoría** de todas las operaciones de escritura sobre las entidades anteriores.

## Mapa de endpoints por dominio

| Dominio | Base path | Documentación |
|---|---|---|
| Autenticación | `/api/auth` | [API → Autenticación](../api/autenticacion.md) |
| Usuarios | `/api/usuarios` | [API → Usuarios](../api/usuarios.md) |
| Transportistas | `/api/transportistas` | [API → Transportistas](../api/transportistas.md) |
| Pedidos | `/api/pedidos` | [API → Pedidos](../api/pedidos.md) |
| Cargas | `/api/cargas` | [API → Cargas](../api/cargas.md) |
| Documentos personales | `/api/documentos` | [API → Documentos personales](../api/documentos.md) |
| Auditoría | `/api/auditoria/**` | [API → Auditoría](../api/auditoria.md) |

## En esta sección

- [Capas y paquetes](capas-paquetes.md)
- [Modelo de dominio](modelo-dominio.md)
- [Seguridad y JWT](seguridad.md)
- [Auditoría](auditoria.md)
- [Manejo de errores](manejo-errores.md)
- [Configuración](configuracion.md)
