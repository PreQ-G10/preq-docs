# Filtrado de Imágenes con DJL — Investigación y Opciones

## Contexto

La idea es filtrar imágenes que no sean productos antes de que lleguen a la base de datos. Ya tengo DJL (`ai.djl:api:0.31.1`) y ONNX Runtime en el proyecto, así que quiero quedarme dentro de ese ecosistema sin agregar dependencias externas pesadas ni APIs pagas.

---

## Opción 1: Clasificación Zero-Shot con CLIP

CLIP (Contrastive Language–Image Pretraining) es un modelo de OpenAI que aprendió a asociar imágenes con texto. La diferencia con un clasificador normal es que no estás atado a etiquetas fijas, vos definís las categorías en tiempo de inferencia pasando prompts de texto. Por eso se llama zero-shot, el modelo nunca vio tus categorías específicas pero las entiende porque generaliza desde el lenguaje.

En la práctica le pasás prompts como `"una foto de producto para una tienda online"` o `"una selfie de una persona"` y CLIP te devuelve un score para cada uno. El más alto es la predicción.

Para este caso eso significa que puedo escribir en texto plano qué es y qué no es un producto, y cambiarlo cuando quiera sin reentrenar nada.

DJL lo soporta a través de la integración con HuggingFace PyTorch. Corre en CPU sin drama aunque a escala la GPU va a hacer una diferencia grande.

**Fortalezas:** No necesito mantener listas de etiquetas, entiende contexto (un producto dentro de una escena, alguien usando el producto, etc.), y para iterar solo cambio el texto de los prompts.

**Debilidades:** Hay que agregar la dependencia de HuggingFace tokenizers por separado. La primera carga es lenta porque baja los pesos del modelo (~600MB para `clip-vit-base-patch32`). Y hay que preprocesar las imágenes a 224x224 antes de mandárselas.

**Ideal para:** Proyectos donde la definición de "producto" puede cambiar con el tiempo o es difícil de meter en una lista fija de etiquetas.

---

## Opción 2: Detección de Objetos con SSD o YOLO via DJL

Los modelos de detección de objetos como SSD o YOLO no clasifican la imagen entera, detectan objetos individuales dentro de ella y te devuelven bounding boxes con etiquetas. DJL los tiene en su model zoo, específicamente variantes de `ssd_512_resnet50_v1_voc` y `yolo` entrenadas en COCO y VOC.

La lógica de filtrado cambia bastante. En lugar de preguntar "¿qué es esta imagen?", preguntás "¿qué objetos tiene?" y en base a eso decidís. Si solo detecta `person`, `sky`, `tree` o `dog` la rechazás. Si detecta `laptop`, `chair`, `bottle` o `keyboard` la aceptás.

Esto funciona mejor que clasificación para imágenes donde el producto aparece en contexto, por ejemplo alguien usando una campera o una laptop sobre un escritorio. Un clasificador capaz etiquete eso como "interior" o "home office", pero el detector igual encuentra la laptop.

**Fortalezas:** Soporte nativo en DJL sin dependencias extra, más rápido que CLIP en CPU, maneja mejor las escenas donde hay varios objetos mezclados.

**Debilidades:** Sigue atado al conjunto fijo de COCO/VOC (80 y 20 clases), que no cubre todo lo que puede tener un e-commerce. Igual hay que mantener un mapeo de qué etiquetas son "producto" y cuáles no, aunque es mucho más chico y estable que las 1000 de ImageNet.

**Ideal para:** Casos donde los productos son objetos comunes que COCO/VOC ya cubre bien.

---

## Opción 3: Clasificación ImageNet con Agrupación por Synsets

El approach más directo, sin configuración adicional porque el model zoo de DJL ya trae clasificadores ImageNet listos (ResNet, MobileNet, EfficientNet). El modelo clasifica la imagen completa en una de las 1000 categorías de ImageNet.

El truco para que esto sea viable, en vez de hacer matching por strings de etiquetas, es usar synset IDs. Son identificadores estables de la taxonomía WordNet sobre la que está construido ImageNet. Por ejemplo `n03642806` siempre es laptop computer sin importar cómo esté escrita la etiqueta. Agrupás los synset IDs en producto y no-producto una sola vez y listo.

El problema real es que ImageNet no fue pensado para e-commerce. Cubre bien objetos cotidianos pero tiene poca cobertura para ropa, cosméticos, accesorios y ese tipo de cosas. Una remera blanca sobre fondo blanco capaz clasifica como `jersey` si tenés suerte, pero también puede tirar cualquier cosa.

**Fortalezas:** Cero dependencias extra, la inferencia más rápida de todas las opciones, menor uso de memoria, funciona offline desde el día uno.

**Debilidades:** Frágil para todo lo que ImageNet no cubre bien, hay que construir y mantener la agrupación de synsets, y la precisión en ropa y lifestyle es bastante mala.

**Ideal para:** Prototipos rápidos o catálogos donde casi todo es un objeto cotidiano que ImageNet conoce bien.

---

## Opción 4: Clasificador Binario Fine-Tuneado

Todas las opciones anteriores usan modelos pre-entrenados de propósito general. Si esto se vuelve algo crítico, la solución más precisa a largo plazo es fine-tunear un modelo chico específicamente para la tarea: producto vs. no producto, con mis propios datos.

DJL soporta entrenamiento y fine-tuning con sus APIs `TrainingConfig` y `Trainer`. La idea es tomar un backbone pre-entrenado como ResNet-50, reemplazar la última capa con una salida de dos clases, y fine-tunear sobre un dataset etiquetado de imágenes de productos e imágenes rechazadas. Una vez entrenado el modelo es chico, rápido y calibrado exactamente para lo que necesito.

El problema es la data. Necesitás un dataset balanceado, unos pocos miles de imágenes de cada lado como mínimo, y el tiempo para armar el pipeline de entrenamiento. No es algo que se hace en un fin de semana, pero cuando está hecho supera a cualquier approach zero-shot o genérico.

**Fortalezas:** Mayor precisión, modelo de inferencia más chico, completamente offline, ajustado exactamente a los tipos de productos del proyecto.

**Debilidades:** Necesitás data etiquetada, trabajo de ML pipeline, y si el catálogo cambia mucho eventualmente hay que reentrenar.

**Ideal para:** Cuando ya tengo suficiente historial de uploads reales para construir el dataset y la precisión es crítica para el producto.

---

## Conclusiones

La opción más sólida para una primera implementación es CLIP zero-shot. No requiere data de entrenamiento, la definición de "producto" se escribe en lenguaje natural y se puede ajustar sin tocar el modelo, y el tradeoff del tamaño y la primera carga se resuelve inicializando el predictor una sola vez al arrancar el servicio.

Si CLIP resulta demasiado lento en el hardware disponible, detección de objetos es el fallback más razonable — más liviano, soporte nativo en DJL, y maneja bien las escenas donde el producto aparece en contexto.

ImageNet con synsets no vale la pena salvo que haya alguna restricción que descarte las otras dos. La lógica de matching de etiquetas se convierte en un problema de mantenimiento bastante feo con el tiempo.

El fine-tuning es el camino a largo plazo. Una vez acumulada suficiente data real de uploads, incluyendo los rechazados, construir un clasificador binario sobre eso es lo más preciso y el modelo resultante termina siendo más liviano que CLIP.
