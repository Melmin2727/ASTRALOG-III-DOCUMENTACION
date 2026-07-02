# Manual del usuario

Esta guía describe cómo usar AstraLog III desde la perspectiva de los dos perfiles de usuario final: **Administrador** (panel web) y **Transportista** (app móvil).

## Roles y dónde operan

| Rol | Plataforma principal | Qué puede hacer |
|---|---|---|
| **Administrador (`ADMIN`)** | Panel web (Angular) | Gestionar transportistas, registrar documentos personales, crear y consultar pedidos, consultar cargas. |
| **Transportista (`TRANSPORTISTA`)** | App móvil Android | Ver su perfil, gestionar su carga (si es `CAMIONERO`) y ver sus pedidos asignados. |

## Panel de administración web

### Iniciar sesión

1. Acceder a la URL del panel (por defecto, `http://localhost:4200` en desarrollo).
2. Ingresar usuario y contraseña en la pantalla de login.
3. Tras un login exitoso, el sistema redirige automáticamente a **Transportistas**.

!!! tip "Si el login falla"
    El formulario de login no muestra actualmente un mensaje de error explícito en pantalla si las credenciales son incorrectas (la lógica de manejo de error no está implementada en `LoginComponent` — ver [Discrepancias](../anexos/discrepancias.md)). Si no ocurre la redirección tras enviar el formulario, verificar usuario y contraseña.

### Gestionar transportistas

En la sección **Transportistas**:

1. Hacer clic en el botón para mostrar el formulario de alta.
2. Completar: nombre, apellidos, DNI (8 dígitos), edad (mínimo 18), tipo de transporte (`CAMIONERO`/`VOLQUETERO`), placa, información del vehículo y capacidad.
3. Al guardar, el sistema **crea automáticamente una cuenta de usuario** para ese transportista:
      - **Usuario:** `primernombre.primerapellido` (en minúsculas; si ya existe, se agrega un número al final).
      - **Contraseña inicial:** el **DNI** del transportista.
4. Seleccionar un transportista de la lista para ver y agregar sus **documentos personales** (SOAT, revisión técnica, licencia, tarjeta de circulación, DNI).

!!! warning "Comunicar la contraseña inicial de forma segura"
    Como la contraseña inicial de cada transportista es su propio DNI, se recomienda como práctica operativa indicarle que **cambie su contraseña** tan pronto como inicie sesión por primera vez en la app móvil, aunque el sistema actualmente no ofrece una pantalla de cambio de contraseña ni lo exige automáticamente.

### Registrar documentos personales

Para cada tipo de documento existen reglas distintas:

| Tipo de documento | Campos requeridos |
|---|---|
| SOAT / Revisión técnica | Fecha de emisión y fecha de vencimiento **obligatorias** |
| Licencia / Tarjeta de circulación | Campo "valor" debe ser `SI` o `NO` |
| DNI | Sin reglas de fecha especiales |

Solo puede existir **un documento de cada tipo** por transportista; para "renovar" un documento (p. ej. un nuevo SOAT vencido), actualmente no hay una opción de edición en el panel web — contactar al equipo técnico, ya que esta operación requiere acceso directo a la API (ver [Discrepancias](../anexos/discrepancias.md)).

### Crear y consultar pedidos

En la sección **Pedidos**:

1. Completar los datos del cliente (nombre, teléfono, dirección de envío).
2. Elegir tipo de transporte; la lista de materiales disponibles cambia según el tipo:
      - `CAMIONERO`: `PANDERETA`, `TECHO`.
      - `VOLQUETERO`: `ARENA_GRUESA`, `ARENA_FINA`, `ARENA_ASENTAR`, `PIEDRA`, `DESMONTE`.
3. Seleccionar el transportista (la lista se filtra automáticamente por el tipo elegido).
4. Indicar cantidad, monto total, adelanto, piso de entrega y hora de envío.
5. Guardar: si el transportista es `CAMIONERO`, el sistema descontará automáticamente la cantidad del inventario de carga de ese transportista (si no hay stock suficiente, el pedido será rechazado).

### Consultar cargas

La sección **Cargas** permite **únicamente consultar y buscar** (por nombre de transportista) el inventario actual de cada transportista camionero. El registro y actualización de inventario se realiza desde la **app móvil** del propio transportista.

### Cerrar sesión

Usar el botón **"Cerrar sesión"** en la barra superior, disponible en todas las pantallas autenticadas.

## Aplicación móvil (transportistas)

### Iniciar sesión

Abrir la app, ingresar usuario y contraseña (los mismos generados/asignados por el administrador) y tocar el botón de ingreso.

### Ver perfil

Tras el login, la pantalla principal muestra los datos del transportista: nombre, DNI, tipo de transporte, placa, vehículo, capacidad y estado.

### Gestionar carga (solo transportistas `CAMIONERO`)

Desde el perfil, acceder a **Carga**:

- Si aún no hay carga registrada, se puede dar de alta indicando el tipo de material (`PANDERETA` o `TECHO`) y la cantidad disponible.
- Si ya existe carga registrada, se puede **aumentar** la cantidad disponible del mismo material.
- Los transportistas `VOLQUETERO` no tienen acceso a esta funcionalidad (el sistema lo bloquea con un mensaje informativo).

### Ver pedidos asignados

Desde el perfil, acceder a **Pedidos** para ver el listado de pedidos asignados al transportista autenticado. Esta pantalla es de **solo consulta**.

### Cerrar sesión

Desde la pantalla de perfil, usar la opción de cerrar sesión, que elimina el token guardado en el dispositivo y regresa a la pantalla de login.

## Página publicitaria

El sitio público (`astramacoiiitransportistas@gmail.com` / `916 859 627`) es de solo lectura para visitantes externos: presenta la empresa, sus servicios, ubicación y datos de contacto. No requiere inicio de sesión ni interactúa con el resto del sistema — ver [Página publicitaria](../pagina-publicitaria.md).
