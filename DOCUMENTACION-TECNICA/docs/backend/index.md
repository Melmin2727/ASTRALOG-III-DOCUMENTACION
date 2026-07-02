# Backend

**Repositorio**: `Backend-AstramacoIII` · **Rama oficial**: `final-trabajo`

## Stack técnico

- **Java 17**, **Spring Boot 3.3.5** (`spring-boot-starter-parent`).
- `spring-boot-starter-web`, `spring-boot-starter-data-jpa`, `spring-boot-starter-security`,
  `spring-boot-starter-validation`.
- **springdoc-openapi-starter-webmvc-ui 2.5.0** — documentación interactiva vía Swagger UI en
  `/swagger-ui.html`, especificación OpenAPI en `/v3/api-docs`.
- **JWT**: `jjwt-api`, `jjwt-impl`, `jjwt-jackson` 0.11.5.
- **MySQL**: `mysql-connector-j` (driver de conexión en runtime).
- **Lombok 1.18.34** para reducción de código boilerplate (getters/setters/builders).
- **Pruebas**: JUnit 5 (`junit-jupiter`), Mockito, `spring-security-test`,
  `spring-boot-testcontainers`, H2 y MySQL como dependencias de test.
- **Pruebas de carga**: k6 (`backend/k6/*.js`), con escenarios de humo (`smoke-test.js`),
  carga (`load-test.js`, `carga-test.js`), estrés (`stress-test.js`) y pruebas específicas por
  dominio (`pedido-test.js`, `usuario-test.js`, `transportistas-test.js`,
  `documentoPersonal-test.js`).
- **Análisis estático**: perfil `Sonar_way` (`backend/conf/Sonar_way.json`) y reporte CNES
  (`tools/sonar-cnes-report-4.3.0.jar`).

## Organización del código

```text
com.example.backendastramaco
├── config/
├── controller/            # Endpoints de dominio
│   └── auditoria/          # Endpoints de auditoría (solo lectura)
├── dto/                   # Objetos de transferencia de datos
├── exception/             # Manejo de excepciones
├── model/                 # Entidades JPA
│   ├── audit/               # Entidades de auditoría
│   └── enums/                # Enumeraciones de dominio
├── repository/            # Repositorios Spring Data JPA
│   └── audit/
├── security/               # Seguridad y JWT
│   ├── config/
│   ├── dto/
│   ├── jwt/
│   ├── service/
│   └── swagger/
└── service/                # Lógica de negocio
    └── audit/
```

## Configuración de la aplicación

Definida en `application.yml` mediante variables de entorno con valores por defecto seguros
para desarrollo local:

| Propiedad | Variable de entorno | Valor por defecto |
|---|---|---|
| URL de base de datos | `DB_URL` | `jdbc:mysql://localhost:3306/astramaco_db` |
| Usuario de base de datos | `DB_USERNAME` | `root` |
| Contraseña de base de datos | `DB_PASSWORD` | *(vacío)* |
| Usuario administrador inicial | `APP_ADMIN_USERNAME` | `admin` |
| Contraseña de administrador inicial | `APP_ADMIN_PASSWORD` | *(requerida)* |
| Secreto JWT | `APP_JWT_SECRET` | *(requerida, sin valor por defecto)* |
| Expiración del token JWT | `APP_JWT_EXPIRATION_MS` | `86400000` (24 h) |
| Puerto del servidor | — | `8080` |

`spring.jpa.hibernate.ddl-auto=update`: el esquema de la base de datos se actualiza
automáticamente a partir de las entidades JPA en cada arranque.

Ver también: [Referencia de API](api-reference.md) · [Modelo de datos](modelo-datos.md) ·
[Seguridad](seguridad.md)
