# Entrega 1 - Documentación

## Resumen Ejecutivo

### Qué se agregó/modificó en esta iteración

Durante esta primera entrega se construyó sobre la primer versión desarrollada para el POC. Se implementó la identificación de usuarios pensando en features de proximas entregas que lo requieren, además se continuó trabajando sobre mas maneras de presentar el análisis de precios a los usuarios, como carritos.
Se implementaron las siguientes funcionalidades:

- **Autenticación de usuarios**, incluyendo login y logout.
- **Visualización de información del usuario**, permitiendo acceder y editar datos personales relevantes desde la aplicación.
- **Escaneo de productos mediante código de barras**, habilitando el reconocimiento de productos de manera rápida por códigos EAN-13, como manera complementaria al escaneo de imágenes.
- **Visualización de información del producto**, screen mostrando datos relevantes obtenidos a partir del producto identificado.
- **Carrito de compras**, permitiendo agregar y gestionar productos seleccionados.
- **Corrección de errores de ubicación**, resolviendo inconsistencias detectadas durante el uso inicial de la aplicación, relacionada a la compatibilidad Android/IOS.

### Decisiones tomadas

#### Uso de escaneo por código de barras

Se optó por implementar el ingreso de productos mediante escaneo de código de barras y esto genero inconvenientes, mas que nada para productos que estaban en la base de datos porque se habian agregado de otra manera previamente pero sin el código de barras, generando duplicados en la base de datos. Elegimos optar por consolidar estas colisiones con una lógica aparte.

#### Cambios de prioridad - Registro de usuarios

Durante inicios de la E1, unos de los features mas importantes para comenzar a implementar era la validación de precios para evitar que agentes maliciosos alteraran la aplicación. Pensando en esto, una de las bases necesarias para continuar con estos features en el futuro fué la identificación de usuarios para trackear usuarios maliciosos y reducir fricción para buenos usuarios.


### Desafíos técnicos encontrados

- Evitar duplicados en BDD al momento de crear nuevos productos.
- Manejo de permisos del dispositivo (cámara y ubicación).

---

## User Stories

### US1 - Escaneo de código de barras

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero poder escanear un código de barras para poder identificar un producto.

#### Valor aportado
Permite identificar productos de forma rápida y reducir la necesidad de búsqueda manual.

#### Criterios de aceptación
- En la pantalla de inicio debe haber un botón
- Al tocar el botón se debe abrir la cámara
- Al apuntar a un código de barras la camara se cierra y se deben obtener los detalles del producto correspondiente al codigo de barras escaneado
- Me debe permitir ajustar los detalles del producto que puedan estar mal o inconsistentes

---

### US9 - Visualización de información de producto al escanear código de barras

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero visualizar la información de un producto luego de escanear un código de barras.

#### Valor aportado
Permite acceder rápidamente a información relevante del producto identificado.

#### Criterios de aceptación
- Al escanear el código de barras se abre una nueva pantalla
- La nueva pantalla contiene la información básica del producto y una solicitud de confirmación si es el correcto

---

### US10 - Añadir productos al carrito

#### Actor/es
- Usuario logueado

#### Funcionalidad
Como usuario quiero poder añadir productos de diferentes negocios a mi carrito para consultarlos posteriormente.

#### Valor aportado
Permite agrupar productos seleccionados dentro de la aplicación en un nuevo objeto que brinda mas información.

#### Criterios de aceptación
- Al buscar un producto, por cualquiera de los metodos disponibles, quiero poder tocar un botón para agregar o quitar una unidad del producto al carrito
- Al volver a la pagina principal, quiero tener un botón que me permita ver mi carrito completo, solo viendo unidades de cada producto
- En la pagina de mi carrito, debo poder agregar o quitar productos (solo de los que ya seleccioné)

---

### US12 - Eliminar productos del carrito

#### Actor/es
- Usuario logueado

#### Funcionalidad
Como usuario, quiero eliminar productos del carrito para poder modificar mis selecciones.

#### Valor aportado
Permite mayor control sobre los productos agregados.

#### Criterios de aceptación
- En la pagina de mi carrito, debo poder descartar totalmente mi carrito y dejarlo vacío

---

### US16 - Login de usuarios

#### Actor/es
- Usuario registrado

#### Funcionalidad
Como usuario quiero poder registrarme y loguearme con credenciales a PreQ

#### Valor aportado
Permite acceder a funcionalidades personalizadas y mantener información asociada al usuario.

#### Criterios de aceptación
- Al abrir PreQ lo primero que quiero ver es una pantalla de login simple que me permita ingresar mis credenciales
- Luego de ingresar mis credenciales, debo tener un botón de “Ingresar” que valide mis credenciales y me permita acceder al resto de la aplicación
- Luego del login, lo primero que quiero ver es la página principal

---

### US17 - Registro de usuarios

#### Actor/es
- Usuario no registrado

#### Funcionalidad
Como usuario, quiero poder registrarme por primera vez a PreQ con credenciales

#### Valor aportado
Permite incorporar nuevos usuarios al sistema.

#### Criterios de aceptación
- Al ingresar a PreQ, lo primero que quiero ver es una página de login
- La página de login debe tener un botón “Registrarme por primera vez” que me lleve a la página de registro
- Debe haber un formulario de registro que me permita ingresar:
  - Nombre
  - Apellido
  - Dirección (opcional)
  - Email
  - Contraseña
- Al ingresar todos los datos requeridos, debo tener un boton de “Registrarme”
- Al clickear en “Registrarme”, debe guardar mi credenciales y llevarme a la página de login, para poder ingresar a la aplicación por primera vez

---

### US19 - Información de usuario logueado

#### Actor/es
- Usuario logueado

#### Funcionalidad
Como usuario quiero tener una página para ver mi información como usuario logueado.

#### Valor aportado
Permite consultar información relevante de la cuenta del usuario.

#### Criterios de aceptación
- En la parte superior derecha de la página home quiero tener un botón que me permita navegar a una nueva página
- La nueva página debe mostrar mi información como usuario logueado y me debe permitir editar cualquier campo

---

### Bug2 - Detección automática de ubicación no existente

#### Descripción
Se corrigió un problema relacionado con la detección automática de ubicación en Android.

#### Resultado esperado
- El sistema maneja correctamente escenarios sin ubicación disponible.
- La aplicación evita fallos o comportamientos inesperados.
- Se provee un fallback adecuado cuando no se detecta ubicación.
