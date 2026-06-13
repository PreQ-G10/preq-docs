# Entrega 3 - Documentación

## Resumen Ejecutivo

### Qué se agregó/modificó en esta iteración

En esta tercera entrega el foco estuvo puesto en mejorar la experiencia de uso del usuario, profundizar el sistema de confiabilidad de precios y seguir fortaleciendo la calidad de los datos dentro de la plataforma.

Se extendió el sistema de validación comunitaria con nuevas señales visuales sobre la confiabilidad de los precios, se incorporaron funcionalidades que acercan la aplicación a un flujo de compra más completo, y se trabajó sobre la robustez del backend refactorizando el servicio de precios.

Se implementaron las siguientes funcionalidades:

- **Marca de verificación para reportes de precios**, mostrando visualmente el nivel de confiabilidad de cada precio reportado.
- **Distribución de precios de un producto**, incorporando un gráfico interactivo en la pantalla de detalle.
- **Visualización de precio sin inflación**, permitiendo comparar el valor estimado sin ese ajuste.
- **Ofertas cerca de la ubicación del usuario**, mostrando en la pantalla principal oportunidades de compra cercanas con buena confianza.
- **Disputa de barcode**, permitiendo reportar cuando un código de barras está asociado al producto equivocado.
- **Agregar al carrito desde búsqueda**, sin necesidad de entrar al detalle del producto.
- **Ver listado de precios de una ubicación**, permitiendo analizar todos los reportes asociados a un comercio específico.
- **Refactorización del PriceService** en el backend.
- **Testeo de frontend**.

Como spike de investigación se exploró la detección de imágenes no correspondientes a productos (S3), relevando opciones técnicas viables dentro del ecosistema DJL para una implementación futura.

### Decisiones tomadas

#### Jest y Vitest en frontend

Se optó por una combinación de ambos frameworks de testing para verificar lógica y correcta composición de páginas. Además se incorporó este nuevo flow como un check del CI existente.

#### Rediseño de la disputa de campos de productos

Como mejora de UX, se decidió modificar el workflow para la disputa de campos de productos, generando un flujo mucho mas amigable para el usuario al momento de reportar un campo malicioso.

Esto fue incluido como parte del refactor para la inclusión de disputa de barcode.

#### Spike de detección de imágenes no correspondientes a productos

Se realizó una investigación sobre las opciones disponibles dentro del ecosistema DJL para detectar imágenes que no pertenezcan a productos antes de persistirlas. El objetivo fue relevar la viabilidad técnica y definir el approach más adecuado para una implementación en una entrega futura, sin llegar a implementar la solución todavía.

Esta investigación comenzó como una exploración de los posibles frameworks, pero se decidió focalizar la investigación en DJL, ya que es una herramienta actualmente acoplada al producto, en el feature de detección de imágenes.

### Desafíos técnicos encontrados

- Implementación del gráfico de distribución de precios. Decidiendo la mejor librería para minimizar riesgos de compatibilidad, pero obteniendo la suficiente flexibilidad para generar gráficos funcionales.

---

## User Stories

### US39 - Marca de verificación para reportes de precios

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero que los precios de los productos tengan una marca de verificación si el precio reportado es confiable.

#### Valor aportado
Permite identificar rápidamente qué precios son confiables sin necesidad de analizar los reportes en detalle.

#### Criterios de aceptación
- En los precios listados debe aparecer un ícono que marque la confiabilidad del precio reportado.
- Un precio con score mayor o igual al 90% debe mostrar una marca verde de precio confiable.
- Un precio con score entre 75% y 90% debe mostrar una marca amarilla de precio dudoso.
- Un precio con score menor al 75% debe mostrar una marca roja de precio poco confiable.
- Al tocar el ícono debe mostrarse una burbuja con un mensaje informativo sobre el indicador.

---

### US36 - Distribución de precio de un producto

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero tener un gráfico en la página de detalles de precio de producto que me permita ver la distribución de los precios.

#### Valor aportado
Permite entender el rango de variación de precios de un producto de forma visual e intuitiva.

#### Criterios de aceptación
- Debajo del detalle de precios debe mostrarse el gráfico de distribución.

---

### US15 - Visualizar precio sin inflación

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder visualizar el precio estimado de un producto sin ponderar la inflación.

#### Valor aportado
Permite comparar el valor real del producto descontando el efecto inflacionario del período analizado.

#### Criterios de aceptación
- En la página de análisis de precios de un producto debe poder visualizarse el precio estimado sin ponderar inflación.

---

### S3 - Spike: Investigación de detección de imágenes maliciosas

#### Actor/es
- Equipo de desarrollo

#### Funcionalidad
Investigar si existe una manera de detectar imágenes no correspondientes a productos para evitar que lleguen a la base de datos y sean tenidas en cuenta para embeddings.

#### Valor aportado
Sienta las bases técnicas para una implementación futura que proteja la integridad de los datos visuales y los vectores de similitud de la plataforma.

#### Criterios de aceptación
- Relevar las opciones técnicas disponibles dentro del ecosistema DJL.
- Documentar las alternativas encontradas con sus ventajas, limitaciones y viabilidad de implementación.

---

### US35 - Ofertas cerca de mi ubicación

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder ver desde la página principal un conjunto de ofertas cercanas a mi ubicación.

#### Valor aportado
Facilita encontrar oportunidades de compra relevantes sin necesidad de buscar activamente.

#### Criterios de aceptación
- Si el usuario no tiene ubicación configurada en su perfil, no deben mostrarse este tipo de ofertas.
- Si tiene ubicación configurada, en la pantalla principal deben aparecer las primeras 5 oportunidades con posibilidad de scrollear.
- Una oportunidad solo se considera como tal si fue reportada con buena confianza cerca de la ubicación del usuario.
- Debe permitirse agregar alguna de estas oportunidades al carrito directamente desde esa vista.

---

### US29 - Disputar un barcode

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero reportar que un barcode está asociado al producto equivocado para corregir la información.

#### Valor aportado
Permite mantener la integridad de la asociación entre códigos de barras y productos dentro de la plataforma.

#### Criterios de aceptación
- Puede disputarse desde la pantalla de detección cuando el producto detectado no coincide con el real.
- No puede disputarse el mismo barcode dos veces por el mismo usuario.
- Debe mostrarse feedback confirmando que la disputa fue registrada.

---

### US25 - Agregar al carrito desde búsqueda

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero poder agregar productos al carrito sin pasar por la vista detallada del mismo, para agilizar mi decisión de compra.

#### Valor aportado
Reduce la fricción en el flujo de compra para usuarios que ya conocen el producto que buscan.

#### Criterios de aceptación
- En el listado de búsqueda de un producto deben mostrarse a la derecha las opciones de añadir o quitar del carrito.

---

### US27 - Ver listado de precios de una ubicación

#### Actor/es
- Usuario

#### Funcionalidad
Como usuario quiero ver todos los precios reportados de una ubicación para juzgar mejor cuál puede ser el correcto.

#### Valor aportado
Permite analizar el historial de reportes de un comercio específico para tomar decisiones más informadas.

#### Criterios de aceptación
- En los detalles de precios cada ubicación debe poder tocarse para abrir una nueva pantalla.
- Al abrir esa pantalla deben listarse todos los reportes de esa ubicación con su fecha de reporte.
