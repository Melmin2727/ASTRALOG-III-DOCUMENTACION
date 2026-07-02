# Despliegue

!!! warning "Sin infraestructura de contenedores en el código analizado"
    Ninguno de los cuatro repositorios incluye `Dockerfile`, `docker-compose.yml`, manifiestos de Kubernetes ni configuración de CI/CD de despliegue (más allá del `Jenkinsfile` del repositorio no relacionado `PSD-20261G1`, que tampoco aplica a AstraLog III). Las recomendaciones de esta página se basan en **despliegue tradicional** (JVM / Node / hosting estático), no en contenedores. Ver [Discrepancias](anexos/discrepancias.md).

## Backend

### Desarrollo

Ejecución directa con `./mvnw spring-boot:run` contra una instancia local de MySQL — ver [Instalación](instalacion.md).

### Producción (recomendación basada en lo implementado)

1. Generar el artefacto ejecutable:

   ```bash
   ./mvnw clean package -DskipTests
   ```

   Esto produce un JAR ejecutable en `target/` (gracias al plugin `spring-boot-maven-plugin`).

2. Ejecutar el JAR en el servidor, proveyendo las variables de entorno de producción:

   ```bash
   java -jar target/backend-astramaco-0.0.1-SNAPSHOT.jar
   ```

   (con `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `APP_JWT_SECRET`, `APP_ADMIN_USERNAME`, `APP_ADMIN_PASSWORD` exportadas en el entorno del proceso, o gestionadas por el orquestador/proceso supervisor usado, p. ej. `systemd`).

3. Usar un proceso supervisor (`systemd`, `pm2` genérico, o similar) para mantener el proceso activo y reiniciarlo ante fallos, dado que el proyecto no incluye configuración de contenedor ni de orquestación.

4. Colocar el backend detrás de un proxy inverso (Nginx/Apache) si se requiere HTTPS, dado que Spring Boot expone HTTP plano en el puerto `8080` por configuración (`server.port: 8080`).

!!! danger "`ddl-auto: update` en producción"
    El backend usa `hibernate.ddl-auto: update`, lo que significa que el **esquema de base de datos se modifica automáticamente** en cada arranque según las entidades JPA. Esto es razonable para desarrollo, pero en un entorno de producción real se recomienda evaluar una estrategia de migraciones versionadas (Flyway/Liquibase), actualmente **no implementada** en el proyecto.

## Frontend web

### Desarrollo

`ng serve` con recarga en caliente — ver [Instalación](instalacion.md).

### Producción

```bash
ng build
```

El resultado en `dist/` es un conjunto de archivos estáticos (HTML/CSS/JS) que puede servirse desde:

- **GitHub Pages / GitLab Pages:** publicando el contenido de `dist/<nombre-proyecto>/browser` (ajustar `base href` según el subpath del repositorio si aplica).
- **Netlify / Vercel:** configurando el comando de build `ng build` y el directorio de publicación `dist/<nombre-proyecto>/browser`.
- **Servidor Linux/Windows con Nginx/Apache/IIS:** copiando el contenido de `dist/` al `docroot` correspondiente, con una regla de *fallback* a `index.html` para soportar el enrutamiento de Angular (single-page application).

!!! warning "`apiUrl` fijo en build time"
    Como `environment.apiUrl` está hardcodeado a `http://localhost:8080/api`, **es obligatorio editar `src/environments/environment.ts` y recompilar** antes de desplegar a un entorno donde el backend no esté en `localhost:8080`. No existe un mecanismo de configuración en tiempo de ejecución (como variables inyectadas en `index.html`) para esto.

## Aplicación móvil

1. Editar `Constants.BASE_URL` apuntando a la URL pública/de producción del backend (idealmente HTTPS).
2. Generar un APK/AAB firmado para distribución:

   ```bash
   ./gradlew assembleRelease   # APK
   ./gradlew bundleRelease     # Android App Bundle, recomendado para Google Play
   ```

3. El `build.gradle.kts` ya tiene habilitados `isMinifyEnabled` e `isShrinkResources` para el tipo de build `release`, con reglas en `proguard-rules.pro`.
4. Distribuir vía Google Play Store, distribución interna (APK directo) o un servicio de distribución de pruebas, según el proceso operativo de la empresa.

## Página publicitaria

Al ser estática, puede publicarse directamente en:

- **GitHub Pages / GitLab Pages:** publicando el contenido del repositorio tal cual (no requiere build).
- **Netlify / Vercel:** sin comando de build (`Build command: none`, `Publish directory: .`).
- **Cualquier servidor web** (Nginx/Apache/IIS) sirviendo el directorio del repositorio como raíz del sitio.

## Resumen de plataformas de destino soportadas

| Componente | GitHub/GitLab Pages | Netlify / Vercel | Servidor Linux | Servidor Windows |
|---|---|---|---|---|
| Backend | ❌ (requiere runtime Java) | ❌ | ✅ (JAR + JVM) | ✅ (JAR + JVM) |
| Frontend web | ✅ (build estático) | ✅ | ✅ | ✅ |
| App móvil | N/A (distribución vía tienda/APK) | N/A | N/A | N/A |
| Página publicitaria | ✅ | ✅ | ✅ | ✅ |
