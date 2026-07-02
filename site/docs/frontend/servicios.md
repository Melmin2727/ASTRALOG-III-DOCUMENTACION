# Servicios y consumo de API

Todos los servicios usan `HttpClient` inyectado por constructor y exponen `Observable`s (RxJS) consumidos por los componentes vía `.subscribe(...)`.

## `AuthService` (`service/auth.service.ts`)

| Método | Descripción |
|---|---|
| `login(data)` | `POST /api/auth/login` |
| `guardarToken(token)` | Guarda el JWT en `localStorage.setItem('token', ...)` |
| `obtenerToken()` | Lee el JWT de `localStorage` |
| `estaLogueado()` | `!!obtenerToken()` |
| `logout()` | `localStorage.removeItem('token')` |

El componente raíz `App` (`app.ts`/`app.html`) usa `AuthService.estaLogueado()` para alternar entre el layout autenticado (topbar con navegación a Pedidos/Transportistas/Documentos/Cargas) y un `<router-outlet>` simple cuando no hay sesión. El botón **"Cerrar sesión"** del topbar invoca `App.logout()`, que llama a `AuthService.logout()` (limpia el token) y fuerza una recarga dura a `/login` (`location.href = '/login'`), igual que `authGuard`.

## `PedidoService` (`service/pedido.service.ts`)

| Método | Endpoint backend |
|---|---|
| `crear(pedido)` | `POST /api/pedidos` |
| `listar()` | `GET /api/pedidos` |

## `TransportistaService` (`service/transportista.service.ts`)

| Método | Endpoint backend |
|---|---|
| `crear(data)` | `POST /api/transportistas` |
| `listar()` | `GET /api/transportistas` |
| `listarPorTipo(tipo)` | `GET /api/transportistas/tipo/{tipo}` |

## `DocumentoService` (`service/documento.service.ts`)

| Método | Endpoint backend |
|---|---|
| `crear(transportistaId, doc)` | `POST /api/documentos/{transportistaId}` |
| `listar(transportistaId)` | `GET /api/documentos/transportista/{id}` |

## `CargaService` (`service/carga.service.ts`)

| Método | Endpoint backend |
|---|---|
| `listarTodas()` | `GET /api/cargas` |
| `obtenerPorTransportista(id)` | `GET /api/cargas/{transportistaId}` |

!!! warning "El frontend web no permite crear ni aumentar cargas"
    `CargaService` del frontend **solo implementa lecturas** (`listarTodas`, `obtenerPorTransportista`). No hay métodos para `PUT /api/cargas/{id}` ni `POST /api/cargas/{id}/aumentar`, y `CargasComponent` es una pantalla de **solo consulta y búsqueda**. La creación/actualización de cargas, según el código analizado, solo es posible desde la **aplicación móvil** (`CargaScreen`) o directamente contra la API. Ver [Aplicación móvil → Pantallas](../movil/pantallas.md).

## Endpoints del backend no consumidos desde el frontend web

| Endpoint | Motivo |
|---|---|
| `PUT /api/usuarios/{id}`, `DELETE /api/usuarios/{id}` | No existe una pantalla de gestión de usuarios en el frontend web; la creación de usuarios ocurre indirectamente al crear un `Transportista`. |
| `PUT /api/transportistas/{id}`, `DELETE /api/transportistas/{id}` | El frontend solo implementa creación y listado de transportistas. |
| `GET /api/transportistas/me`, `GET /api/pedidos/me` | Usados por la app móvil, no por el panel web. |
| `PUT /api/cargas/{id}`, `POST /api/cargas/{id}/aumentar` | Usados por la app móvil. |
| `GET /api/auditoria/**` | No hay ninguna pantalla de auditoría en el frontend web. |
