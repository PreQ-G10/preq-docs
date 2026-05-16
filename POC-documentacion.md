# PoC - PreQ

## Resumen Ejecutivo

### Qué se agregó/modificó en esta iteración

Durante la PoC (Proof of Concept) del proyecto **PreQ** se desarrollaron las funcionalidades mínimas necesarias para validar la viabilidad técnica del producto y reducir incertidumbre respecto a las decisiones de arquitectura y tecnologías seleccionadas.

En esta etapa se implementaron y validaron:

- **Página inicial de la aplicación**, como punto de entrada para la navegación principal.
- **Visualización básica de información de producto**, para comprobar la capacidad de representar datos relevantes de un producto en la interfaz.
- **Detección automática de ubicación/negocio**, con el objetivo de validar integración con geolocalización y contextualización futura de información.
- **Data initializer (seed de datos)** para facilitar pruebas y permitir disponer de información inicial durante el desarrollo.
- **Spike de reconocimiento de productos basado en imágenes**, para evaluar alternativas al reconocimiento mediante imagenes y analizar viabilidad técnica.

### Decisiones tomadas

#### Evaluación de reconocimiento basado en imágenes

Se investigó la posibilidad de reconocer productos mediante imágenes, como alternativa o complemento al escaneo de códigos de barras, buscando reducir errores al momento de reconocer productos. Esta funcionalidad fue tratada como un spike técnico para determinar su factibilidad.

#### Validación temprana de funcionalidades críticas

Se decidió implementar una prueba de concepto orientada a validar aquellas funcionalidades consideradas de mayor riesgo técnico antes de avanzar con una implementación completa.

#### Creación de datos iniciales para desarrollo

Se decidió incorporar un mecanismo de inicialización de datos para simplificar pruebas funcionales y evitar dependencias tempranas con ambientes productivos o carga manual de datos.

### Desafíos técnicos encontrados

- Evaluación de modelos de deep learning open source para la limpieza del background de las imágenes.
- Integración de geolocalización en dispositivos móviles.
- Creación de datos de prueba consistentes y lo suficientemente variados como para no caer en el reconocimiento del mismo producto.

---

# User Stories

## US5 - Página inicial de aplicación

### Actor/es
- Usuario

### Funcionalidad
Como usuario, quiero acceder a una página inicial para poder comenzar a interactuar con la aplicación.

### Valor aportado
Permite centralizar el acceso a las funcionalidades principales y establecer un punto de entrada consistente para la experiencia del usuario.

### Criterios de aceptación
- Quiero tener un botón que me lleve a poder escanear o detectar un producto
- Quiero tener un buscador que me lleve a buscar el precio de uno o mas productos

---

## US7 - Visualizar información básica de producto

### Actor/es
- Usuario

### Funcionalidad
Como usuario, quiero visualizar información básica de un producto para conocer datos relevantes del mismo.

### Valor aportado
Permite validar la capacidad del sistema para consultar y representar información de productos.

### Criterios de aceptación
- Quiero poder visualizar detalles del producto, como el nombre, marca, cantidad del envase, rango de precios, un top con las ubicaciones con mejores precios para el producto, cantidad de contribuciones con el precio del producto y más

---

## US8 - Detección automática de ubicación/negocio

### Actor/es
- Usuario

### Funcionalidad
Como usuario, quiero que la aplicación detecte automáticamente mi ubicación para contextualizar funcionalidades futuras.

### Valor aportado
Permite personalizar la experiencia y validar funcionalidades geográficas para próximas iteraciones.

### Criterios de aceptación
- Si estoy reconociendo el producto mediante un escaneo o foto, quiero que se autocomplete la ubicación desde la que estoy tomando la foto.
- Si se autocompleta efectivamente la ubicación, quiero poder corregirla, en caso de que este tomando la foto desde otra localización o el autocompletado sufra erroes
- Si decido corregir la ubicacion y crear una nueva ubicación, debe validar la dirección que estoy ingresando, además de mostrarme un mini mapa con la ubicación seleccionada

---

## Spike2 - Reconocimiento de productos basado en imágenes

### Objetivo
Investigar la viabilidad técnica de reconocimiento de productos mediante imágenes como alternativa al escaneo tradicional.

### Resultado esperado
- Documentación breve de las opciones disponibles y criterios usados para la opción elegida
- Añadir a la documentación de decisiones generales el resultado de la investigación
- Realizar un test interno para verificar la efectividad de una o varias soluciones

### Aprendizajes
- Evaluación de procesamiento de imágenes.
- Consideración de uso de almacenamiento de imágenes.
- Consideración de uso de herramientas OCR.
- Definición preliminar de arquitectura técnica.

---

## T5 - Crear data initializer

### Objetivo
Crear un mecanismo para inicializar datos del sistema y facilitar el desarrollo y testing de funcionalidades.

### Valor aportado
Reduce tiempos de setup y mejora la consistencia de pruebas durante el desarrollo.

### Resultado esperado
- Disponibilidad de datos iniciales.
- Simplificación de pruebas funcionales.
- Menor dependencia de carga manual de información.

---

## US2 - Reconocimiento de producto por foto

### Actor/es
- Usuario

### Funcionalidad
Como usuario quiero poder clickear un botón que abra mi camara, me permita capturar la imagen de un producto y me autocomplete la información del producto, permitiendo revisar y corregir si hay errores en la detección.

### Valor aportado
Permite explorar una alternativa más flexible al escaneo tradicional, reduciendo fricción cuando el código de barras no está disponible o no puede ser leído.

### Criterios de aceptación
- UI con un botón que abra la camara del dispositivo
- Al escanear el producto deseado, poder corregir el producto detectado por otro
- Si no se detecto el producto, debe permitirme elegir entre tomar otra foto o buscar manualmente

---

## US6 - Poder contribuir a la base de datos de productos

### Actor/es
- Usuario

### Funcionalidad
Como usuario, quiero poder contribuir a la base de datos de productos para mejorar la cobertura de información disponible en la aplicación.

### Valor aportado
Permite expandir progresivamente la cantidad de productos registrados y reducir faltantes de información en el sistema.

### Criterios de aceptación
- Al escanear un producto, ya sea para reconocerlo y comparar su precio, o por cualquier razón, me gustaría recibir la opción de contribuir a registrar el precio del producto.
- Si decido contribuir con el precio del producto, quiero que se me permita agregar el precio del producto manualmente y guardarlo.
- Si decido contribuir con el precio del producto y lo guardo, quiero continuar con mi flujo normal de usuario
