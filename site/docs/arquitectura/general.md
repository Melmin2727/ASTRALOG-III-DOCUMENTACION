# Arquitectura general

AstraLog III sigue una arquitectura **cliente-servidor desacoplada**: un único backend Spring Boot expone una API REST consumida por tres clientes independientes (web, móvil y, parcialmente, la página publicitaria como sitio estático sin backend propio).

## Componentes y comunicación

```mermaid
flowchart LR
    subgraph Clientes
        FE["Frontend Web<br/>Angular 21"]
        APP["App Móvil<br/>Android / Kotlin / Compose"]
        ADS["Página Publicitaria<br/>HTML/CSS/JS estático"]
    end

    subgraph Servidor
        BE["Backend<br/>Spring Boot 3.3.5 (Java 17)"]
        DB[("Base de datos<br/>MySQL")]
    end

    FE -- "HTTP/JSON + JWT<br/>(Authorization: Bearer)" --> BE
    APP -- "HTTP/JSON + JWT<br/>vía Retrofit" --> BE
    BE -- "JPA / Hibernate" --> DB
    ADS -. "Sin integración con el backend<br/>(sitio informativo independiente)" .-> ADS
```

!!! warning "Página publicitaria sin integración"
    El repositorio `PaginaPublicitaria-AstramacoIII` es un sitio estático (HTML/CSS/JS puro) sin llamadas a la API del backend ni formularios conectados a un servidor. Es exclusivamente informativo/promocional. Esto se documenta también en [Anexos → Discrepancias detectadas](../anexos/discrepancias.md).

## Flujo de autenticación entre componentes

```mermaid
sequenceDiagram
    actor U as Usuario (Admin / Transportista)
    participant C as Cliente (Web o App)
    participant API as Backend (Spring Boot)
    participant DB as MySQL

    U->>C: Ingresa usuario y contraseña
    C->>API: POST /api/auth/login
    API->>DB: Busca usuario por username
    DB-->>API: Usuario + hash BCrypt
    API->>API: Verifica password y genera JWT (HS256)
    API-->>C: 200 OK { token, username, rol }
    C->>C: Guarda token (localStorage / SharedPreferences)
    C->>API: Peticiones siguientes con header Authorization: Bearer {token}
    API->>API: JwtFilter valida el token en cada request
    API-->>C: Respuesta autorizada según el rol
```

## Roles del sistema

El backend define tres roles (`enum Rol`) que condicionan el acceso:

| Rol | Descripción |
|---|---|
| `ADMIN` | Administra usuarios, transportistas, pedidos y cargas. Único rol autorizado a crear usuarios (`POST /api/usuarios`). |
| `USER` | Rol genérico de usuario autenticado. |
| `TRANSPORTISTA` | Asociado 1 a 1 a una entidad `Transportista`; consume los endpoints `/me` para ver su propio perfil, pedidos y carga. |

## Vista de despliegue lógico

```mermaid
flowchart TB
    subgraph "Dispositivo del cliente"
        Browser["Navegador<br/>(Angular SPA servida)"]
        Phone["Smartphone Android<br/>(APK AstraLog)"]
    end

    subgraph "Servidor de aplicación"
        Spring["Spring Boot :8080<br/>/api/**"]
        Swagger["springdoc-openapi<br/>/swagger-ui.html"]
    end

    subgraph "Servidor de datos"
        MySQL[("MySQL<br/>astramaco_db")]
    end

    Browser -->|"REST/JSON"| Spring
    Phone -->|"REST/JSON (Retrofit)"| Spring
    Spring --> MySQL
    Spring --- Swagger
```

## Comunicación entre componentes: resumen técnico

| Origen | Destino | Protocolo | Autenticación | Notas |
|---|---|---|---|---|
| Frontend Angular | Backend | HTTP/JSON sobre `environment.apiUrl` (`http://localhost:8080/api`) | JWT vía `jwtInterceptor` (HttpInterceptorFn) | Token leído de `localStorage` |
| App Android | Backend | HTTP/JSON vía Retrofit (`Constants.BASE_URL`) | JWT enviado manualmente como header `Authorization` en cada llamada de `ApiService` | Token persistido con `TokenManager` (SharedPreferences) |
| Backend | MySQL | JDBC / Hibernate (Spring Data JPA) | Credenciales por variables de entorno | `ddl-auto: update` |
| Página publicitaria | — | N/A | N/A | Sin backend propio; es un sitio estático |
