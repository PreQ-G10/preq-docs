# Entrega 2 - Documentación

## Resumen Ejecutivo

### Qué se agregó/modificó en esta iteración

Durante esta segunda entrega se continuó expandiendo la aplicación enfocándose principalmente en mejorar la validación de información ingresada por usuarios y en reforzar la colaboración comunitaria dentro de la plataforma.

Se incorporaron mecanismos de consenso y reputación para permitir validar datos de productos, imágenes y precios de manera más confiable, además de mejoras importantes en la experiencia de usuario.

Se implementaron las siguientes funcionalidades:

- **Visualización de precios pendientes de validación**, permitiendo a usuarios colaborar validando precios cercanos.
- **Confirmación y disputa de precios**, habilitando validación comunitaria sobre reportes realizados por otros usuarios.
- **Disputa de campos de productos**, permitiendo corregir nombre, marca y cantidades incorrectas.
- **Carga de nuevas imágenes de productos**, permitiendo ampliar la base visual de productos existentes.
- **Mejoras en búsqueda de productos**, mostrando imágenes y rangos de precios aproximados.
- **Navegación directa desde resultados rápidos**, reduciendo pasos innecesarios en búsquedas.
- **Mejoras en el flujo de imágenes**, incluyendo validación por consenso y expiración.

### Decisiones tomadas

#### Uso de trust score para validaciones

Se optó por incorporar un sistema de reputación mediante trust score para reducir acciones maliciosas y mejorar la confiabilidad de validaciones comunitarias.

Este valor permite ponderar:

- Confirmaciones de precios.
- Disputas.
- Reportes realizados.
- Participación de usuarios.

La intención fue evitar que usuarios nuevos o maliciosos puedan alterar fácilmente la información mostrada en la aplicación.

#### Consenso para validación de imágenes

Se decidió no aprobar automáticamente las imágenes subidas por usuarios.

En cambio, se implementó un sistema de consenso utilizando embeddings y cosine similarity para detectar si una imagen realmente pertenece al producto asociado.

Esto permite:

- Evitar imágenes incorrectas.
- Detectar spam.
- Mantener consistencia visual entre productos.

#### Sistema de disputas comunitarias

Se optó por permitir disputas tanto para precios como para campos de productos.

La intención fue construir un sistema colaborativo donde los usuarios puedan mejorar información incorrecta sin depender exclusivamente de moderación manual.

### Desafíos técnicos encontrados

- Manejo de consenso para validaciones comunitarias.
- Implementación de cosine similarity utilizando embeddings.
- Queries complejas para agrupamientos y conteos.
- Optimización de búsquedas mostrando información agregada de precios.
- Manejo de validaciones evitando acciones duplicadas de usuarios.
- Manejo de imágenes pendientes y expiración.

---

## User Stories

### US14 - Ver precios pendientes de validación

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero ver en la pantalla principal si hay precios pendientes de validación para ayudar a la comunidad.

#### Valor aportado
Permite incentivar la colaboración comunitaria y mejorar la calidad de los precios registrados.

#### Criterios de aceptación
- Si existen precios pendientes cercanos, debe mostrarse un banner en la pantalla principal.
- Si no existen precios pendientes, no debe mostrarse nada.
- Al tocar el banner, debe navegarse a la pantalla de validación.
- No deben mostrarse precios previamente validados por el usuario.
- Solo pueden validar usuarios con trust score suficiente.

---

### US28 - Confirmar o disputar un precio

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero confirmar o disputar un precio reportado para ayudar a la comunidad.

#### Valor aportado
Permite validar colaborativamente precios y detectar información incorrecta.

#### Criterios de aceptación
- Debe poder confirmarse un precio desde la pantalla de validación.
- No debe poder confirmarse o disputarse el mismo precio más de una vez.
- Si el usuario disputa un precio, debe poder ingresar un nuevo valor.
- Si no se ingresa un valor alternativo, la disputa no debe registrarse.
- Debe mostrarse feedback indicando que la acción fue registrada.

---

### US30 - Disputar un campo de producto

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario, quiero corregir información incorrecta de un producto.

#### Valor aportado
Permite mejorar la calidad de los datos mostrados dentro de la aplicación.

#### Criterios de aceptación
- Debe poder disputarse nombre, marca, cantidad y tipo de cantidad.
- Debe ingresarse el nuevo valor correcto.
- No debe poder disputarse el mismo campo más de una vez.
- Debe mostrarse feedback indicando que la disputa fue registrada.

---

### US32 - Agregar nuevas imágenes de productos

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder agregar nuevas imágenes de productos.

#### Valor aportado
Permite ampliar la información visual disponible de productos.

#### Criterios de aceptación
- Desde la pantalla de detalle debe existir un botón para agregar imágenes.
- Al tocar el botón debe abrirse la cámara.
- Deben solicitarse permisos si son necesarios.
- Luego de tomar la foto debe poder confirmarse o repetirse.
- Al guardar debe mostrarse un mensaje de confirmación.

---

### US24 - Ver información ampliada en resultados de búsqueda

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero visualizar más información de productos en los resultados de búsqueda.

#### Valor aportado
Permite identificar productos más rápidamente sin ingresar al detalle.

#### Criterios de aceptación
- En los resultados de búsqueda debe mostrarse:
  - Imagen del producto.
  - Precio mínimo.
  - Precio máximo.
- La información debe visualizarse de forma clara.

---

### US33 - Navegación directa desde búsqueda rápida

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero acceder directamente al detalle de un producto desde resultados rápidos.

#### Valor aportado
Reduce pasos innecesarios y mejora la experiencia de navegación.

#### Criterios de aceptación
- Al tocar un resultado rápido debe abrirse directamente la pantalla de detalle del producto.
