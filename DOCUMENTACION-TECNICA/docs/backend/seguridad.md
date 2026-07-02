# Seguridad

## Autenticación

- Basada en **Spring Security** + **JWT** (`jjwt` 0.11.5).
- `POST /api/auth/login` recibe un `AuthRequest` (username/password), delega en
  `AuthenticationManager` y, si la autenticación es exitosa, `JwtUtil.generateToken()` firma
  un token con clave HMAC (`Keys.hmacShaKeyFor`) derivada de `app.jwt.secret`.
- El token incluye `subject` (username), `issuedAt` y `expiration`
  (`app.jwt.expiration-ms`, 24 h por defecto).
- `JwtFilter` intercepta las peticiones entrantes, valida el token (`JwtUtil.validateToken`)
  y extrae el username (`JwtUtil.extractUsername`) para autenticar el contexto de seguridad.

## Autorización

`SecurityConfig` define la política de acceso:

- **CSRF deshabilitado** (`csrf.disable()`) — coherente con una API stateless consumida por
  clientes JWT.
- **CORS habilitado** vía `corsConfigurationSource()` / `CorsConfig`, para permitir el
  consumo desde el Frontend Web y la Aplicación Móvil.
- Un conjunto de rutas configuradas con `permitAll()` (por ejemplo, login y documentación
  Swagger).
- Regla específica: `POST /api/usuarios` requiere el rol `ADMIN`
  (`.requestMatchers(HttpMethod.POST, "/api/usuarios").hasRole("ADMIN")`).
- **Todas las demás rutas requieren autenticación** (`anyRequest().authenticated()`).

## Roles del sistema

| Rol | Uso previsto |
|---|---|
| `ADMIN` | Gestión completa, incluida la creación de usuarios. |
| `USER` | Usuario estándar (gerencia / operaciones logísticas). |
| `TRANSPORTISTA` | Acceso desde la Aplicación Móvil a sus propios pedidos, cargas y documentos (`/me`). |

## Gestión del token en los clientes

- **Frontend Web (Angular)**: `AuthService.guardarToken()` almacena el JWT en
  `localStorage`; `authGuard` protege las rutas `pedidos`, `transportistas`, `documentos` y
  `cargas` en `app.routes.ts`.
- **Aplicación Móvil (Android)**: `TokenManager` (paquete `data/local`) gestiona la
  persistencia del token en el dispositivo.

## Trazabilidad y auditoría

Toda operación relevante sobre `Usuario`, `Pedido`, `Carga`, `Transportista` y
`DocumentoPersonal` queda registrada en el módulo de auditoría (`service/audit`,
`repository/audit`, `model/audit`), consultable en modo solo lectura vía
`/api/auditoria/**`. Este mecanismo responde directamente al objetivo **OE9 – Seguridad** del
Project Charter de Auditoría SDLC, que exige verificar "la efectividad de los mecanismos de
autenticación, autorización, gestión de roles, protección de datos y trazabilidad".

## Administrador inicial

El backend soporta la creación de un usuario administrador inicial mediante variables de
entorno (`APP_ADMIN_USERNAME`, `APP_ADMIN_PASSWORD`), sin credenciales embebidas en el
código fuente ni valores por defecto para la contraseña — evitando así un riesgo común de
credenciales hardcodeadas.

!!! info "Relación con la Auditoría SDLC"
    El Informe Final de Auditoría SDLC concluye una opinión favorable sobre la
    implementación correcta de mecanismos de autenticación y autorización (ver el sitio
    independiente **Auditoría SDLC → Hallazgos**), consistente con lo observado directamente
    en `SecurityConfig` y `JwtFilter`.
