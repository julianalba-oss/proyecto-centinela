# ☕ Proyecto Centinela: Sistema de Alerta Temprana en Caficultura

**Propuesta de Datos y Desarrollo Integral — Fases 1 y 2**  
*Maestría en Ciencia de Datos | Asignatura: Deep Learning*  
*Universidad Santo Tomás*

---

## 👥 Equipo de Trabajo
* **Andrés Julián Alba Gualteros**
* **Angie Camila Rojas**
* **Andrea Olarte Trujillo**

📅 **Fecha:** Noviembre de 2026  
📌 **Estado:** Fases 1 y 2 Completadas ✅

---

## 🎯 1. Problema y Cliente
**Centinela** es un sistema de alerta temprana diseñado para detectar hojas de café afectadas por **roya del cafeto** (*Hemileia vastatrix*) y **ácaro rojo** (*Oligonychus yothersi*).

* **Cliente objetivo:** Pequeños caficultores.
* **Propuesta de valor e impacto:** La detección oportuna permite al agricultor decidir si debe inspeccionar a fondo su lote o aplicar tratamientos focalizados. Esto reduce pérdidas de cosecha, optimiza costos en insumos agrícolas y mitiga el impacto ambiental derivado de fumigaciones preventivas a ciegas.

### 🧭 Síntesis Ejecutiva del Proyecto
* **Fase 1 (Línea base tabular):** Se construyó una primera línea de comparación sencilla mediante un Perceptrón Multicapa (**MLP**) entrenado sobre 12 variables estadísticas de color para establecer un punto de partida cuantitativo (**AUC = 0.639**).
* **Fase 2 (Arquitecturas profundas y multimodalidad):** Se evaluaron redes neuronales profundas (**CNN** para visión por computador, redes recurrentes para series climáticas y una arquitectura de **fusión multimodal**) para comprobar si el contexto meteorológico mejora el diagnóstico visual.
* **Conclusión central:** El análisis visual mediante redes convolucionales funciona de manera sobresaliente (**AUC = 0.93**), mientras que la variable climática, bajo la estructura temporal de este conjunto de datos, no aporta ganancia predictiva.

---

## ⚙️ 2. Arquitectura de Datos y Modalidades

### 🌐 Visión General del Sistema (Multimodal)
El sistema integra dos fuentes de información exploradas progresivamente a lo largo del proyecto:
1. **Visión por Computador (Imagen):** Fotografías RGB de hojas de café.
2. **Series Temporales (Clima):** Variables meteorológicas diarias de la zona del cultivo (temperatura, humedad relativa y precipitación), obtenidas mediante la API abierta *NASA POWER*.

### 📐 Enfoque de la Fase 1 (Representación Tabular y Línea Base)
Para esta primera etapa, la modalidad de **imagen se aplanó hacia una estructura tabular** mediante ingeniería de características manual:
* **Variables predictoras (12 características):** Media (μ) y desviación estándar (σ) de los canales de color **R, G, B** y **H, S, V** extraídas de cada fotografía.
* **Variable objetivo:** Clasificación binaria (`0`: Hoja sana; `1`: Hoja enferma por ácaro rojo o roya en niveles de severidad 1 a 4).
* **Justificación de la línea base:** Un modelo simple (**MLP**) actúa como vara de medir rigurosa; si una arquitectura profunda posterior no supera este umbral, la complejidad adicional no se justifica.

### 🧠 Evolución en Fase 2 (CNN, Series Temporales y Fusión)
1. **Visión por Computador (CNN):** Se compararon dos arquitecturas preentrenadas en ImageNet (**ResNet50** y **EfficientNet-B0**) bajo dos estrategias: *Feature Extraction* (entrenando solo la cabeza clasificadora) y *Fine-Tuning* (ajustando las capas profundas). Dado que se dispone de 1.092 imágenes de entrenamiento, el aprendizaje por transferencia evita la memorización y aprovecha extractores previos de bordes y texturas.
2. **Modelado Climático (RNN, LSTM, GRU):** Se formuló la predicción de humedad relativa del día siguiente usando una ventana histórica de 14 días, comparando arquitecturas recurrentes frente a reglas base estadísticas (**Persistencia** y **ARIMA**).
3. **Fusión Multimodal (Imagen + Clima):** Se concatenaron las representaciones latentes de la **CNN** y la **GRU** para evaluar si el clima mejora la alerta temprana, incorporando un experimento de control negativo con variables climáticas barajadas al azar.

---

## 📊 3. Fuente de Datos y Originalidad

* **Dataset base:** Parraga-Alava, J., Cusme, K., Loor, A. y Santander, E. (2019). *RoCoLe: A robusta coffee leaf images dataset* (Versión 2). Mendeley Data. [https://doi.org](https://doi.org)
* **Licencia:** CC BY 4.0 (Uso libre con atribución).
* **Trazabilidad y reproducibilidad:** Se documentaron el DOI, la licencia, las fechas de descarga y las huellas criptográficas **SHA-256** de los archivos. Las afirmaciones no verificables directamente en la plataforma (como la ausencia de sumas de verificación publicadas por archivo en Mendeley) quedaron registradas explícitamente como supuestos metodológicos.
* **Volumen y origen:** 1.560 fotografías provenientes de 390 plantas de café robusta (4 capturas por planta), recolectadas en Calceta, Manabí (Ecuador), con etiquetas clínicas de estado y severidad.

### 🛡️ Justificación de Originalidad
1. **Fuente académica rigurosa:** Proviene de un conjunto de investigación validado y publicado en *Data in Brief*, ajeno a repositorios de competencias estándar.
2. **Pertinencia temática:** El dominio caficultor responde directamente a problemáticas agrícolas reales contempladas en la asignatura.
3. **Auditoría e ingeniería propia:** Tanto la extracción tabular de color (RGB/HSV) como la depuración de duplicados exactos y la partición libre de fuga constituyen aportes originales del equipo.

---

## 🛠️ 4. Preparación de Datos, Resultados y Lecciones Aprendidas

### 🔍 Auditoría de Datos y Partición Libre de Fuga (70 / 15 / 15)
* **Agrupamiento por planta (`plant_id`):** Dado que las 4 fotos de una misma planta comparten fondo e iluminación, dividir aleatoriamente permitiría que el modelo memorice la planta en lugar de la patología.
* **Hallazgo de fotos duplicadas (Auditoría humano-IA):** Aunque una partición inicial sugerida por IA parecía razonable, la revisión humana detectó **67 pares de fotografías idénticas (134 archivos)** registrados con nombres distintos. En 37 de esos pares, una copia quedaba en entrenamiento y otra en prueba.
* **Corrección (`particion_semilla42_v2.csv`):** Se reconstruyó la partición garantizando que tanto las plantas como todos los grupos de duplicados quedaran aislados en un mismo subconjunto, reutilizando esta matriz exacta en la Fase 2. Tras corregir la fuga, el AUC del MLP pasó de `0.686` a `0.639` (con intervalos de confianza solapados, lo que sugiere que la cifra inicial pudo estar inflada).
* **Ruido de etiquetas documentado:** Se identificó una pareja de imágenes casi idénticas con etiquetas contradictorias (`C11P23E1` y `C12P36E1`); al ser irresoluble clínicamente, se declaró explícitamente como ruido de etiquetado.
* **Preprocesamiento:** Todo escalado y normalización se ajustó (`fit`) exclusivamente sobre el conjunto de entrenamiento. Las series climáticas se dividieron de forma estrictamente cronológica para evitar filtrar información futura.

### 📈 Resultados Experimentales por Fase

| Fase / Componente | Modelo / Estrategia | Métrica Principal (Test) | Hallazgo Clave |
| :--- | :--- | :--- | :--- |
| **Fase 1 (Tabular)** | **MLP** (12 variables RGB/HSV) | **AUC: 0.639** | Línea base validada tras eliminar fuga por duplicados. |
| **Fase 2 (Imagen)** | **EfficientNet-B0** (*Fine-Tuning*) | **AUC: 0.93** / **F1 macro: 0.73** (3 clases) | Supera ampliamente la línea base; seleccionado por F1 en validación. |
| **Fase 2 (Clima)** | **RNN / LSTM / GRU** vs. **ARIMA** | **RMSE: 2.61–2.68** vs. **ARIMA: 2.60** | Las redes no superan a ARIMA por alta autocorrelación diaria. La GRU conserva ~40x más gradiente lejano que la RNN simple. |
| **Fase 2 (Fusión)** | **EfficientNet-B0 + GRU** | **Δ AUC: +0.000** (`[-0.003, +0.003]`) | El clima no aporta señal discriminante; el control con clima aleatorio arroja el mismo resultado. |

### 💡 Lecciones Aprendidas (Síntesis Metodológica)
1. **Una métrica alta no es evidencia sin auditar los datos:** Revisar la partición permitió descubrir duplicados ocultos y evitar conclusiones optimistas falsas.
2. **Las líneas base protegen contra la complejidad innecesaria:** Sin comparar contra ARIMA, se habría reportado una red recurrente que no aporta ganancia real frente a un modelo estadístico clásico.
3. **Un resultado negativo bien medido es un hallazgo válido:** El clima no mejoró la clasificación porque las 1.560 fotos fueron capturadas en apenas **5 días distintos** (5 ventanas climáticas para todo el dataset). El clima distingue un día de otro, pero no diferencia una hoja enferma de una sana dentro del mismo día.
4. **Los intervalos de confianza guían la interpretación:** Con muestras acotadas, variaciones de pocas centésimas no constituyen evidencia concluyente.
5. **Supervisión crítica sobre herramientas de IA:** Se documentó en bitácora qué sugerencias de IA fueron aceptadas, modificadas o rechazadas tras la verificación humana.

---

## 📂 5. Entregables y Estructura del Repositorio

| Recurso | Descripción | Acceso |
| :--- | :--- | :--- |
| **Propuesta Técnica (PDF)** | Documento formal con el planteamiento, alcance y metodología de la Fase 1. | [📄 Ver PDF](./documentos/Propuesta_de_Datos_Centinela_Fase1.pdf) |
| **Dataset Procesado (CSV)** | Matriz tabular con las 12 características de color calculadas y etiquetas binarias. | [📊 Descargar CSV](./documentos/dataset_centinela.csv) |
| **Paquete Completo Fase 1 (ZIP)** | Datos tabulares, partición depurada (`particion_semilla42_v2.csv`), informe técnico-ético y bitácora de IA. | [📦 Descargar Fase 1](./documentos/Centinela_Fase1.zip) |
| **Fase 2 (ZIP) - (notebook ejecutado en Colab - Informe Técnico - Bitácora de IA)** | Experimentos CNN (ResNet50, EfficientNet-B0, Grad-CAM), series climáticas, fusión multimodal y reportes. | [📦 Descargar Fase 2](./documentos/Centinela_Fase2.zip) |
| **Fase 2 — Carpeta Figuras** | Gráficas de entrenamiento, curvas ROC/PR, matrices de confusión y mapas de calor Grad-CAM. | [🖼️ Ver Figuras (OneDrive)](PEGA_AQUI_ENLACE_FIGURAS) |
| **Fase 2 — Carpeta Pesos** | Modelos entrenados y *checkpoints* serializados (CNN, GRU y arquitectura de fusión multimodal). | [⚖️ Descargar Pesos (OneDrive)](PEGA_AQUI_ENLACE_PESOS) |

```text
Proyecto-centinela/
├── documentos/
│   ├── Propuesta_de_Datos_Centinela_Fase1.pdf
│   ├── dataset_centinela.csv
│   ├── Centinela_Fase1.zip
│   └── Centinela_Fase2.zip
└── README.md
```

---

## ⚖️ 6. Riesgos Éticos, Limitaciones y Mitigación

* **Falsos Negativos (Riesgo crítico):** En el mejor modelo visual (**EfficientNet-B0**), aproximadamente **16 de cada 100 hojas enfermas** aún no son detectadas, concentrándose los errores en **roya incipiente**. El monitoreo de sensibilidad (*Recall*) sigue siendo prioritario para evitar falsa seguridad en el caficultor.
* **Atención espuria en fondo (Interpretabilidad):** Los mapas de calor (**Grad-CAM**) evidenciaron que en ciertos casos la red atiende al fondo de la fotografía en lugar de la lesión foliar, lo que motiva segmentación o recorte previo en futuras iteraciones.
* **Falsos Positivos:** Alertas erróneas implican gastos innecesarios en fungicidas/acaricidas y contaminación ambiental evitable.
* **Sesgo de cobertura geográfica, varietal y temporal:** Al entrenarse con café *Robusta* capturado en condiciones controladas durante 5 días en Manabí (Ecuador), la generalización a café *Arábica*, a iluminación abierta de campo o a dinámicas climáticas multianuales requiere recolección de datos longitudinales.
* **Privacidad de datos:** El conjunto de datos contiene exclusivamente registros botánicos y ambientales; no involucra datos personales ni información sensible.
* **Marco ético y gobernanza:** El diseño se alinea con el *Marco Ético para la Inteligencia Artificial en Colombia* (Guío Español et al., 2021). **Centinela** opera estrictamente como un sistema de apoyo a la decisión (*Human-in-the-Loop*), manteniendo el criterio agronómico y la decisión final en manos del caficultor.
