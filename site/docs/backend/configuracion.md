# Configuración

## `application.yml`

```yaml
spring:
  datasource:
    url: ${DB_URL:jdbc:mysql://localhost:3306/astramaco_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true}
    username: ${DB_USERNAME:root}
    password: ${DB_PASSWORD:}
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: true

app:
  admin:
    username: ${APP_ADMIN_USERNAME:admin}
    password: ${APP_ADMIN_PASSWORD:}
  jwt:
    secret: ${APP_JWT_SECRET}
    expiration-ms: ${APP_JWT_EXPIRATION_MS:86400000}

server:
  port: 8080
  address: 0.0.0.0

springdoc:
  api-docs:
    path: /v3/api-docs
    enabled: true
  swagger-ui:
    path: /swagger-ui.html
    enabled: true
    tryItOutEnabled: true
    filter: true
```

## Variables de entorno

| Variable | Obligatoria | Valor por defecto | Descripción |
|---|---|---|---|
| `DB_URL` | No | `jdbc:mysql://localhost:3306/astramaco_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true` | URL JDBC de conexión a MySQL. |
| `DB_USERNAME` | No | `root` | Usuario de la base de datos. |
| `DB_PASSWORD` | No | *(vacío)* | Contraseña de la base de datos. |
| `APP_ADMIN_USERNAME` | No | `admin` | Username del usuario administrador inicial creado por `DataInitializer`. |
| `APP_ADMIN_PASSWORD` | **Sí, para crear el admin inicial** | *(vacío → se omite la creación)* | Contraseña del administrador inicial. Si no se define, el sistema arranca sin crear ningún usuario `ADMIN` y registra una advertencia en logs. |
| `APP_JWT_SECRET` | **Sí** | *(sin valor por defecto)* | Clave secreta HMAC usada para firmar y validar los JWT. La aplicación fallará al iniciar si no está definida, dado que no tiene fallback en el YAML. |
| `APP_JWT_EXPIRATION_MS` | No | `86400000` (24 h) | Tiempo de expiración del token JWT, en milisegundos. |

!!! danger "`APP_JWT_SECRET` sin valor por defecto"
    A diferencia del resto de variables, `app.jwt.secret: ${APP_JWT_SECRET}` no define un valor de respaldo. **Es obligatorio exportar `APP_JWT_SECRET` antes de iniciar el backend**, o la aplicación fallará al arrancar (`JwtUtil.init()` lanzará una excepción al intentar construir la clave HMAC con un valor nulo).

## Dependencias Maven relevantes para configuración

Ver el detalle completo de versiones en [Arquitectura → Tecnologías utilizadas](../arquitectura/tecnologias.md). El backend gestiona sus dependencias con **Maven** (`pom.xml`), heredando versiones del `spring-boot-starter-parent:3.3.5` para la mayoría de starters, y fijando explícitamente las versiones de `springdoc-openapi`, `jjwt-*`, `lombok`, `testcontainers` y los plugins de build (`jacoco`, `surefire`, `failsafe`, `sonar-maven-plugin`).

## Sobre Docker

!!! warning "No se encontró configuración de Docker en el backend"
    Pese a que el prompt original solicitaba documentar "Docker (si existe)", **no se encontró ningún `Dockerfile` ni `docker-compose.yml`** en `Backend-AstramacoIII` ni en ningún otro repositorio del ecosistema. El despliegue documentado en [Instalación](../instalacion.md) y [Despliegue](../despliegue.md) se basa exclusivamente en ejecución directa con Maven/JVM. Ver también [Discrepancias](../anexos/discrepancias.md).

## Pruebas de carga (k6)

El backend incluye scripts de **k6** (`/k6`) para pruebas de carga, humo (*smoke*) y estrés sobre los dominios de usuario, pedido y transportista:

```
k6/
├── usuario/ {load-test.js, smoke-test.js, stress-test.js}
├── pedido/  {load-test.js, smoke-test.js, stress-test.js}
└── transportista/ {load-test.js, smoke-test.js, stress-test.js, diagnostico.js}
```

Estos scripts no forman parte del runtime de producción; son herramientas de validación de rendimiento ejecutadas externamente con `k6 run <script>.js`, apuntando a una instancia del backend ya desplegada.
