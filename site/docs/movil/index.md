# Aplicación móvil — Visión general

La aplicación móvil (`AppAstraLog-AstramacoIII`, paquete `com.example.astralog`) es una **app nativa Android** dirigida a los **transportistas**: les permite autenticarse, ver su perfil, gestionar su carga (si son `CAMIONERO`) y consultar sus pedidos asignados.

- **Lenguaje:** Kotlin.
- **UI:** Jetpack Compose + Material 3.
- **Arquitectura:** MVVM (Model-View-ViewModel) con `Repository` como capa de acceso a datos.
- **Cliente HTTP:** Retrofit + OkHttp (con `HttpLoggingInterceptor` para depuración).
- **Persistencia local:** `SharedPreferences` (vía `TokenManager`), solo para el JWT.
- **`applicationId`:** `com.example.astralog` · **`minSdk`:** 24 · **`targetSdk`/`compileSdk`:** 36.

!!! info "Esta es la app que da nombre a \"AstraLog\""
    De los cuatro repositorios, `AppAstraLog-AstramacoIII` es el único cuyo paquete y nombre de proyecto usan literalmente "AstraLog" (`com.example.astralog`); los demás repositorios usan el nombre comercial "Astramaco"/"ASTRAMACO III".

## Configuración de conexión al backend

```kotlin
object Constants {
    const val BASE_URL = "http://192.168.0.102:8080/api/"
    // cambiar la dirección IP de la red a donde está apuntando la PC y el celular
}
```

!!! warning "IP del backend hardcodeada"
    La URL base del backend está **fija en el código fuente** (`Constants.BASE_URL`) apuntando a una IP de red local específica (`192.168.0.102`), con un comentario que indica que debe cambiarse manualmente según la red de desarrollo. No hay un mecanismo de configuración por entorno (build variants, `BuildConfig`, etc.) para `debug`/`release`. Esto significa que **para que la app funcione fuera de esa red local específica, es obligatorio editar y recompilar el código fuente**. Ver [Instalación](../instalacion.md) y [Discrepancias](../anexos/discrepancias.md).

## En esta sección

- [Arquitectura Android](arquitectura.md)
- [Pantallas y navegación](pantallas.md)
