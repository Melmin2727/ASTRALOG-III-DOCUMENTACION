# Frontend — Visión general

El frontend (`Frontend-AstramacoIII`) es una **SPA Angular 21** standalone (sin `NgModule`, usando `bootstrapApplication` y componentes `standalone: true`) que funciona como panel de administración para la operación diaria de ASTRAMACO III.

- **Servidor de desarrollo:** `ng serve` → `http://localhost:4200`.
- **API consumida:** `http://localhost:8080/api` (`environment.apiUrl`, hardcodeado para desarrollo).
- **Gestor de estado:** ninguno (sin NgRx/Signals store); el estado vive en propiedades de cada componente.
- **Pruebas:** Vitest + jsdom (`ng test`).

## Páginas / rutas disponibles

| Ruta | Componente | Protegida por `authGuard` |
|---|---|---|
| `/login` | `LoginComponent` | No |
| `/` | Redirige a `/login` | — |
| `/pedidos` | `PedidosComponent` | Sí |
| `/transportistas` | `TransportistasComponent` | Sí |
| `/documentos` | `DocumentosComponent` | **No** (ver discrepancia) |
| `/cargas` | `CargasComponent` | **No** (ver discrepancia) |

!!! warning "Rutas `/documentos` y `/cargas` sin guard de autenticación"
    A diferencia de `/pedidos` y `/transportistas`, las rutas `/documentos` y `/cargas` están declaradas en `app.routes.ts` **sin** `canActivate: [authGuard]`. Cualquier usuario, autenticado o no, puede navegar directamente a esas rutas en el navegador (aunque las llamadas HTTP que disparen seguirán siendo rechazadas por el backend con `401` si no hay JWT válido). Ver [Discrepancias](../anexos/discrepancias.md).

## En esta sección

- [Arquitectura Angular](arquitectura.md)
- [Routing y guards](routing.md)
- [Servicios y consumo de API](servicios.md)
