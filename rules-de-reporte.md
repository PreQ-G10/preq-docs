# Lógica de precios — preq
 
Este documento explica cómo funciona el sistema de precios de preq: cómo se reportan, cómo se validan, cómo se calculan y cómo se protegen contra datos maliciosos.
 
---
 
## ¿Cómo se reporta un precio?
 
Cuando un usuario reporta un precio, el sistema aplica una serie de reglas automáticas antes de guardarlo. Todo esto ocurre de forma invisible para el usuario.
 
### Paso 1 — ¿Hay suficientes datos? (Arranque en frío)
 
Si un producto tiene menos de 3 reportes previos, el sistema no tiene una referencia confiable para comparar. En ese caso, el precio se acepta directamente sin validación adicional y se marca como "pocos datos" en la interfaz.
 
### Paso 2 — ¿El precio es razonable? (Umbral de desvío)
 
Si hay suficientes datos, el sistema calcula un **precio promedio ponderado** (ver más abajo) y verifica si el nuevo precio se aleja más del 40% de ese promedio.
 
- Si el desvío es mayor al 40% → el reporte se guarda pero se marca como **inválido** y no afecta el precio mostrado.
- Si el desvío es menor al 40% → el reporte continúa al siguiente paso.
### Paso 3 — ¿El usuario estaba cerca? (Coherencia de ubicación)
 
Si el usuario envió su ubicación GPS al reportar, el sistema verifica si estaba a menos de 200 metros del local. Si estaba más lejos, el reporte recibe un **puntaje de confianza de ubicación** reducido proporcionalmente a la distancia. Esto no invalida el reporte, pero le da menos peso al calcular el precio.
 
### Paso 4 — Cálculo del puntaje del reporte
 
Cada reporte recibe un **puntaje entre 0 y 1** que determina su estado:
 
```
puntaje = confianza_ubicación × (0.5 + puntaje_usuario × 0.5) × (1 - desvío_penalidad × 0.3)
```
 
Este puntaje combina tres factores:
- **Confianza de ubicación** — ¿estaba el usuario cerca del local?
- **Puntaje de confianza del usuario** — ¿tiene historial confiable?
- **Desvío del precio** — ¿qué tan lejos está del promedio?
Los reportes se clasifican según su puntaje:
 
| Puntaje | Estado | Efecto |
|---|---|---|
| ≥ 0.7 | **Válido** | Se muestra y afecta el precio |
| 0.4 – 0.7 | **Pendiente de revisión** | Se muestra pero espera validación comunitaria |
| < 0.4 | **Inválido** | Se guarda pero no afecta el precio mostrado |
 
---
 
## ¿Cómo se calcula el precio mostrado?
 
El precio mostrado en la app es un **promedio ponderado con decaimiento temporal**. Esto significa que:
 
- Los reportes recientes pesan más que los antiguos.
- Los reportes con baja confianza de ubicación pesan menos.
- Solo se incluyen reportes con puntaje **válido** (≥ 0.7).
Esto permite que el precio se ajuste automáticamente a la inflación sin necesidad de una fuente externa, ya que los reportes más recientes naturalmente reflejan los precios actuales.
 
---
 
## ¿Qué es la validación comunitaria?
 
Cuando un reporte queda en estado **pendiente de revisión** (puntaje entre 0.4 y 0.7), otros usuarios de confianza cercanos pueden confirmarlo o disputarlo.
 
### ¿Quién puede validar?
 
Solo usuarios con un **puntaje de confianza ≥ 0.25** y que estén cerca del local donde se reportó el precio (en los últimos 15 días).
 
### Confirmar un precio
 
Si un usuario de confianza confirma el reporte, el puntaje del reporte sube proporcionalmente al puntaje de confianza del confirmador:
 
```
nuevo_puntaje = puntaje + (1 - puntaje) × confianza_confirmador × 0.3
```
 
Esto puede llevar el reporte de "pendiente" a "válido".
 
### Disputar un precio
 
Si un usuario considera que el precio es incorrecto, puede indicar el precio correcto que vio en el local. Esto tiene tres efectos:
 
1. El puntaje del reporte original **baja** proporcionalmente a la diferencia de precio.
2. El puntaje de confianza del reportero original **baja** proporcionalmente a la diferencia.
3. El precio alternativo del disputador se **guarda como un nuevo reporte** y pasa por el proceso de ingestion normal.
---
 
## ¿Qué es el puntaje de confianza del usuario?
 
Cada usuario tiene un **puntaje de confianza entre 0 y 1** (por defecto 0.5 al registrarse). Este puntaje sube o baja según su comportamiento:
 
**Sube cuando:**
- Un reporte es aceptado: `+0.01 × multiplicador_de_recuperación`
- Confirma un reporte pendiente válido: `+0.01`
**Baja cuando:**
- Un reporte es rechazado por desvío: `−desvío²`
- Disputa un reporte con diferencia de precio: penalidad proporcional a la diferencia
El puntaje siempre está entre 0 y 1.
 
### Multiplicador de recuperación
 
Si el puntaje de confianza cae por debajo de 0.25, la recuperación se vuelve más lenta. Cada vez que esto ocurre, el multiplicador de recuperación se divide a la mitad:
 
| Caída | Recuperación por reporte aceptado |
|---|---|
| Primera vez | +0.01 |
| Segunda vez | +0.005 |
| Tercera vez | +0.0025 |
 
### Auto-exclusión silenciosa
 
Si el puntaje de un usuario cae por debajo de 0.25:
- Sus reportes se guardan pero no afectan el precio mostrado hasta que otro usuario de confianza los confirme.
- No puede confirmar ni disputar reportes de otros.
- El usuario **nunca es notificado** de este estado.
