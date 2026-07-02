# Instalación

Cada componente de AstraLog III se instala y ejecuta de forma independiente. No existe un script u orquestador único (como `docker-compose`) que levante todo el ecosistema de una vez — ver nota en [Discrepancias](anexos/discrepancias.md).

## Requisitos generales

| Componente | Requisitos |
|---|---|
| Backend | JDK 17, Maven (o usar el wrapper `mvnw` incluido), MySQL 8.x en ejecución |
| Frontend web | Node.js (compatible con Angular 21 / Angular CLI 21.2.6) y npm |
| App móvil | Android Studio reciente, JDK 17, SDK de Android (`compileSdk`/`targetSdk` 36, `minSdk` 24) |
| Página publicitaria | Ninguno (HTML/CSS/JS estático); opcionalmente un servidor HTTP simple para previsualizar |

## 1. Backend (`Backend-AstramacoIII`)

### 1.1. Preparar la base de datos

```bash
# Crear la base de datos en MySQL (si no existe)
mysql -u root -p -e "CREATE DATABASE astramaco_db;"
```

### 1.2. Configurar variables de entorno

=== "Linux / macOS"

    ```bash
    export DB_URL="jdbc:mysql://localhost:3306/astramaco_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true"
    export DB_USERNAME="root"
    export DB_PASSWORD="tu_password"
    export APP_ADMIN_USERNAME="admin"
    export APP_ADMIN_PASSWORD="defineUnaContraseñaSegura"
    export APP_JWT_SECRET="defineUnSecretoLargoYAleatorioDeAlMenos32Caracteres"
    export APP_JWT_EXPIRATION_MS="86400000"
    ```

=== "Windows (PowerShell)"

    ```powershell
    $env:DB_URL="jdbc:mysql://localhost:3306/astramaco_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true"
    $env:DB_USERNAME="root"
    $env:DB_PASSWORD="tu_password"
    $env:APP_ADMIN_USERNAME="admin"
    $env:APP_ADMIN_PASSWORD="defineUnaContraseñaSegura"
    $env:APP_JWT_SECRET="defineUnSecretoLargoYAleatorioDeAlMenos32Caracteres"
    $env:APP_JWT_EXPIRATION_MS="86400000"
    ```

!!! danger "`APP_JWT_SECRET` es obligatorio"
    Sin esta variable, el backend **no arrancará** — ver [Backend → Configuración](backend/configuracion.md).

### 1.3. Compilar y ejecutar

```bash
cd Backend-AstramacoIII

# Con el wrapper de Maven incluido (recomendado, no requiere Maven instalado)
./mvnw clean install        # Linux/macOS
mvnw.cmd clean install      # Windows

./mvnw spring-boot:run      # Linux/macOS
mvnw.cmd spring-boot:run    # Windows
```

El backend quedará disponible en `http://localhost:8080`, con Swagger UI en `http://localhost:8080/swagger-ui.html`.

Al arrancar por primera vez (con `APP_ADMIN_PASSWORD` definida), `DataInitializer` creará automáticamente el usuario `ADMIN` inicial.

## 2. Frontend web (`Frontend-AstramacoIII`)

```bash
cd Frontend-AstramacoIII
npm install
npm start          # equivalente a: ng serve
```

La aplicación quedará disponible en `http://localhost:4200`, apuntando por defecto a `http://localhost:8080/api` (`src/environments/environment.ts`).

!!! tip "Apuntar a un backend en otra URL"
    Editar `src/environments/environment.ts` y cambiar `apiUrl` antes de compilar, ya que el proyecto no usa variables de entorno de Node/build en tiempo de ejecución para esta configuración.

### Compilación de producción

```bash
ng build
```

Los artefactos quedan en `dist/`.

## 3. Aplicación móvil (`AppAstraLog-AstramacoIII`)

1. Abrir el proyecto en **Android Studio**.
2. **Antes de compilar**, editar `app/src/main/java/com/example/astralog/utils/Constants.kt` y actualizar `BASE_URL` con la IP de red local donde corre el backend, accesible desde el dispositivo/emulador:

   ```kotlin
   object Constants {
       const val BASE_URL = "http://<IP_DE_TU_PC>:8080/api/"
   }
   ```

3. Sincronizar Gradle (`Sync Project with Gradle Files`).
4. Ejecutar sobre un emulador o dispositivo físico conectado (`Run 'app'`), o generar un APK:

```bash
cd AppAstraLog-AstramacoIII
./gradlew assembleDebug      # Linux/macOS
gradlew.bat assembleDebug    # Windows
```

El APK resultante queda en `app/build/outputs/apk/debug/`.

!!! warning "Emulador Android y `localhost`"
    Si se usa el emulador de Android Studio, `localhost` del emulador **no** apunta a la máquina host; debe usarse la IP real de red local de la máquina donde corre el backend (o `10.0.2.2` específicamente para el emulador estándar de Android Studio, aunque el código actual no usa este alias — debe configurarse manualmente en `Constants.kt`).

## 4. Página publicitaria (`PaginaPublicitaria-AstramacoIII`)

No requiere instalación. Para previsualizar localmente:

```bash
cd PaginaPublicitaria-AstramacoIII
python3 -m http.server 8000
# o, alternativamente:
npx serve .
```

Luego abrir `http://localhost:8000` en el navegador.

## 5. Documentación (este sitio MkDocs)

El propio `README.md` del proyecto de documentación resume el mismo procedimiento:

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate
# Linux/macOS
source .venv/bin/activate

python -m pip install --upgrade pip
pip install -r requirements.txt

mkdocs serve   # http://127.0.0.1:8000
mkdocs build   # genera el sitio estático en site/
```
