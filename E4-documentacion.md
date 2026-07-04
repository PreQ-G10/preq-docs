# Entrega 4

## Resumen Ejecutivo

### Qué se agregó/modificó en esta iteración

En esta cuarta entrega el foco estuvo puesto en expandir la plataforma hacia una nueva línea de funcionalidades orientadas a **negocios**, sin dejar de seguir fortaleciendo la experiencia del usuario final dentro del flujo de compra.

Se incorporó un conjunto de herramientas para que los comercios puedan registrarse dentro de la plataforma, gestionar su perfil, mantener un catálogo propio y reportar precios de forma más directa. En paralelo, se sumaron mejoras orientadas al usuario para visualizar con mayor claridad la información de precios, construir y administrar listas de compras, y seguir fortaleciendo la calidad de los datos reportados en la aplicación.

Se implementaron las siguientes funcionalidades:

* **Creación de cuenta de negocio**, incorporando un flujo de registro especializado para comercios.
* **Home de negocio**, ofreciendo una pantalla inicial con acceso a las principales funcionalidades disponibles para este tipo de cuenta.
* **Perfil de negocio**, permitiendo visualizar y editar la información registrada del comercio.
* **Catálogo de negocio**, habilitando la gestión de los productos ofrecidos por cada negocio.
* **Agregar productos al catálogo de negocio**, permitiendo incorporar productos existentes junto con su precio.
* **Crear producto como negocio**, posibilitando dar de alta productos que todavía no existan en la plataforma.
* **Visualización de precios reportados por negocios**, identificando cuándo un precio fue informado directamente por un comercio.
* **Insignia de confiabilidad para precios de negocio**, mostrando visualmente la confiabilidad del precio reportado por un comercio según su actualización y desvío.
* **Detalle de producto en la pantalla de precios**, incorporando información identificatoria del producto en la vista de análisis.
* **Advertencias de precios viejos en catálogo**, ayudando a los negocios a detectar productos con precios desactualizados.
* **Guardado de lista de compras tras armar el carrito**, permitiendo persistir una compra para su seguimiento posterior.
* **Interacción con listas de compras**, habilitando completar ítems individuales o finalizar listas completas.
* **Eliminación de listas de compras**, permitiendo remover listas guardadas que ya no sean necesarias.
* **Denuncia de imágenes de producto**, ofreciendo un mecanismo para reportar imágenes incorrectas, inapropiadas o poco claras.
* **Métricas simples de negocio**, incorporando indicadores básicos en la home del comercio para dar visibilidad sobre la interacción de los usuarios con su negocio.

### Decisiones tomadas

#### Incorporación del actor negocio dentro de la plataforma

Se decidió extender el alcance del sistema incorporando un nuevo tipo de cuenta orientado a comercios, con necesidades y flujos propios. Esto implicó separar la experiencia de negocio de la experiencia del usuario consumidor, habilitando funcionalidades específicas como la administración de catálogo, la gestión de precios y la visualización de métricas.

Esta decisión permitió estructurar una experiencia más alineada con el rol del negocio dentro de la plataforma, sin sobrecargar la navegación del usuario final con herramientas que no le corresponden.

#### Gestión del catálogo como núcleo del flujo de negocio

Se optó por centralizar gran parte de la experiencia del negocio alrededor de su catálogo de productos. A partir de esta decisión, el catálogo pasó a ser el punto principal desde el cual un comercio puede agregar productos, crear nuevos, modificar precios y detectar información desactualizada.

Esto permitió dar coherencia al flujo de gestión del negocio y sentar una base clara para futuras funcionalidades vinculadas a promociones, administración de stock o análisis comercial.

#### Consolidación del flujo de listas de compras

Se decidió completar el flujo iniciado en iteraciones anteriores incorporando la posibilidad de guardar listas de compras, interactuar con ellas una vez creadas y eliminarlas cuando ya no sean útiles. De esta manera, el carrito deja de ser una instancia aislada y pasa a integrarse con una experiencia más persistente de planificación de compra.

Esto mejora la utilidad práctica de la plataforma para usuarios que comparan precios y organizan compras a lo largo del tiempo, en lugar de resolver todo en una única sesión.

#### Refuerzo de la trazabilidad y calidad de la información

Se trabajó también en reforzar la claridad del origen y confiabilidad de la información mostrada. Para eso se diferenciaron visualmente los precios reportados por negocios, se incorporaron insignias específicas para esos reportes y se habilitó la denuncia de imágenes de producto que puedan resultar incorrectas o inapropiadas.

Estas decisiones apuntan a mejorar la confianza del usuario en los datos de la plataforma y a facilitar la detección de contenido desactualizado o de baja calidad.

### Desafíos técnicos encontrados

* Integración de un nuevo tipo de cuenta dentro del sistema existente, manteniendo separados los flujos de usuario y negocio sin duplicar lógica innecesariamente.
* Modelado de la gestión de catálogo de negocio, contemplando alta de productos, asociación de precios y actualización de información desactualizada.
* Definición de criterios de confiabilidad para precios reportados por negocios, considerando tanto la antigüedad del precio como su desvío respecto de reportes comunitarios.
* Extensión del flujo de carrito hacia listas de compras persistentes, incorporando estados, acciones sobre ítems y operaciones de eliminación.
* Diseño de mecanismos de moderación de contenido visual para permitir denuncias de imágenes sin afectar la experiencia de navegación del usuario.

---

## User Stories

### US50 - Métricas simples de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio, quiero tener una sección en la página de home que me permita visualizar métricas simples sobre mi negocio.

#### Valor aportado
Permite al negocio entender rápidamente señales clave de uso y desempeño (guardados recientes, promedio de precios y productos más añadidos), facilitando decisiones comerciales.

#### Criterios de aceptación
- La sección debe mostrar la cantidad de usuarios que guardaron una lista de compras para mi negocio en los últimos 30 días.
- Debe mostrar el precio promedio de las últimas 10 listas de compras.
- Debe mostrar un top con los 5 productos más añadidos a listas de compras.

---

### US34 - Denunciar imagen de producto

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero denunciar imágenes incorrectas/inapropiadas/mal tomadas para mejorar la calidad de la información de los productos.

#### Valor aportado
Mejora la calidad del contenido visual de productos y reduce información engañosa o inapropiada en la plataforma.

#### Criterios de aceptación
- Ver un botón en la esquina superior izquierda de la imagen dentro de detalles de producto.
- Al tocar el botón se abre un modal con un formulario de reporte.
- El formulario debe tener un desplegable para seleccionar el motivo y un campo de descripción.
- Los motivos del reporte pueden ser “Imagen inapropiada/sugestiva” e “Imagen poco clara”.
- Al enviar el reporte se registra y no se puede volver a reportar.
- Si hay muchos reportes diferentes por la misma causa, la imagen se remueve.

---

### US44 - Lista de compras tras carrito

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder guardar mi lista de compras tras armar mi carrito, para poder seguir mi compra de cerca.

#### Valor aportado
Permite persistir el resultado del armado del carrito para seguimiento posterior de compras.

#### Criterios de aceptación
- Luego de armar mi carrito y ver las mejores opciones, debe haber un botón para “Guardar mi lista de compras”.
- Al hacer click, debe llevar a una página de listas de compras donde se vean las históricas y la nueva agregada.

---

### US45 - Interactuar con lista de compras

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder interactuar con mis listas de compras para poder darles un mejor uso.

#### Valor aportado
Permite gestionar el progreso de compra por ítems y por lista completa.

#### Criterios de aceptación
- Debo poder completar ítems específicos para llevar la cuenta de qué productos faltan.
- Debo poder completar la compra entera independientemente de si completé todos los ítems o no.
- Las compras finalizadas deben mostrar una insignia de finalizada, a diferencia de las que siguen en curso.

---

### US46 - Eliminar listas de compras

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder eliminar listas de compras para que no se acumulen.

#### Valor aportado
Ayuda a mantener ordenada la sección de listas y evita acumulación innecesaria.

#### Criterios de aceptación
- Debe aparecer un ícono de eliminar junto a cada lista.
- Al hacer click en eliminar debe aparecer un popup de confirmación.
- Al confirmar debe desaparecer la lista.
- Al cancelar no ocurre nada.

---

### US26 - Creación de cuenta de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio quiero tener una cuenta especializada, para gestionar los precios de mis productos.

#### Valor aportado
Habilita una experiencia orientada a negocios con datos y operaciones específicas.

#### Criterios de aceptación
- En la página de registro debe haber opción para elegir tipo de cuenta negocio.
- Debe solicitar campos: Dirección, Nombre, Email, Contraseña y Tipo de negocio.
- Solo puede haber una cuenta por ubicación de negocio.
- Al registrarme debe guiarme a la página de inicio.

---

### US47 - Home de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio, quiero tener una página de home para visualizar al iniciar sesión o registrarme como negocio.

#### Valor aportado
Ofrece un punto de entrada claro para negocios con contexto inicial de funcionalidades.

#### Criterios de aceptación
- Debe haber una sección explicando las principales funcionalidades disponibles para el negocio.

---

### US49 - Perfil de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio, quiero tener una página que me permita gestionar los datos de mi negocio.

#### Valor aportado
Permite mantener la información del negocio actualizada y controlar la sesión.

#### Criterios de aceptación
- La página debe contener los detalles con los que registré mi negocio y permitir editarlos.
- Debe haber un botón para guardar información y actualizar cambios.
- Debe haber un botón de logout que redirija a login.

---

### US37 - Catálogo de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio quiero poder tener un catálogo para ver y gestionar los productos que ofrezco.

#### Valor aportado
Permite administrar la oferta de productos y su mantenimiento en un único flujo.

#### Criterios de aceptación
- Desde home debe existir un botón para navegar a la página de catálogo.
- Deben existir botones para retirar productos del catálogo o modificar su precio.
- La página debe mostrar el total de productos en catálogo.

---

### US38 - Agregar productos a catálogo de negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio quiero poder agregar nuevos productos a mi catálogo.

#### Valor aportado
Facilita ampliar la oferta del negocio de forma rápida desde la búsqueda.

#### Criterios de aceptación
- Desde la búsqueda de productos debe aparecer un botón para agregar al catálogo.
- Al clickear, debe abrir un popup para seleccionar precio.
- El popup debe permitir confirmar agregado o cancelar.

---

### US42 - Crear producto como negocio

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio quiero poder crear un producto que no exista en la plataforma para mostrar mi catálogo completo.

#### Valor aportado
Permite cubrir faltantes de catálogo y reflejar correctamente la oferta real del negocio.

#### Criterios de aceptación
- En la página del listado de productos del negocio debe haber un botón de creación.
- Al tocarlo debe abrir un formulario de creación de producto con precio para la ubicación del negocio.
- Debe poder aceptarse o cancelarse la creación.
- El nuevo producto debe quedar disponible para agregar al catálogo.

---

### US40 - Ver precio reportado por negocios

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero ver el precio que reportó un negocio sobre un producto para tener una fuente directa de información.

#### Valor aportado
Aporta una fuente adicional y explícita de precios reportados por comercios.

#### Criterios de aceptación
- En la página de detalles de precios debe aparecer en el top si un precio fue reportado por un comercio.
- Junto al precio reportado por el negocio debe aparecer la fecha de reporte.

---

### US41 - Insignia de precio de negocio

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero ver una insignia junto al reporte de precio de un negocio en la lista de negocios en la página de detalles de precio, indicando la confiabilidad del precio reportado por el negocio.

#### Valor aportado
Facilita evaluar rápidamente la confiabilidad del precio informado por un negocio.

#### Criterios de aceptación
- Junto al precio reportado por el negocio debe aparecer una insignia:
  - Roja si el precio del negocio se desvía hacia abajo más de 20% del precio reportado por usuarios o si no se actualiza hace 90 días.
  - Amarilla si se desvía hacia abajo más de 10% o si no se actualiza hace más de 60 días.
  - Verde si no se cumple ninguna de las condiciones anteriores.

---

### US43 - Ver detalle de producto en detalles de precio

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero ver los identificadores de un producto en la página de detalles de precio.

#### Valor aportado
Mejora el contexto del producto al analizar precios, reduciendo ambigüedad.

#### Criterios de aceptación
- Al ingresar a la página de detalles de precio debe aparecer en la parte superior el nombre, marca y cantidad del producto.

---

### US48 - Advertencias de precios viejos

#### Actor/es
- Negocio

#### Funcionalidad
Como negocio quiero visualizar los precios que hayan quedado desactualizados para poder estar al tanto de mi catálogo.

#### Valor aportado
Permite mantener precios actualizados y mejorar la calidad de la información publicada.

#### Criterios de aceptación
- En la página de gestión de catálogo debe aparecer una sección con productos sin actualización hace más de 60 días.
- Cada fila debe tener botón de actualizar precio que abra popup para cargar nuevo precio.
- Cada fila debe tener botón de mantener precio que la marque en color verde.
- Al completar todas las filas, la sección debe desaparecer.