# ☕ Proyecto Centinela: Sistema de Alerta Temprana en Cafetales
**Maestría en Ciencia de Datos** | *Asignatura: Deep Learning*  
*Universidad Santo Tomás*  

---

## 👥 Integrantes
* **Andrés Julián Alba Gualteros**
* **Angie Camila Rojas**
* **Andrea Olarte Trujillo**

📅 **Fecha:** Noviembre de 2026  

---

## 🚀 Resumen de Fases Abordadas

El desarrollo de **Centinela del Café** se ha estructurado en dos etapas progresivas que escalan desde un enfoque tabular clásico hacia una arquitectura profunda multimodal:

* **🔹 Fase 1 — Línea Base Tabular (Completada):** Se transformó el problema de visión artificial a una estructura tabular extrayendo **12 características estadísticas de color** (media y desviación estándar en espacios RGB y HSV) por cada fotografía. Se estableció una estrategia estricta de partición por planta (70/15/15) para evitar fuga de datos (*data leakage*) y se entrenó el modelo base de clasificación binaria (`Sana` vs. `Enferma`).
* **🔹 Fase 2 — Fusión Multimodal Deep Learning (En desarrollo / Abordada):** Evolución hacia el procesamiento directo de **imágenes crudas mediante Redes Neuronales Convolucionales (CNN)**, fusionadas con **series temporales climáticas** (temperatura, humedad y precipitación obtenidas de *NASA POWER*) para capturar tanto los síntomas visuales en la hoja como las condiciones ambientales propicias para el desarrollo de la roya y el ácaro rojo.

---

## 📦 Descarga de Archivos del Proyecto (ZIP)

Para facilitar la reproducibilidad y revisión del trabajo, los entregables y conjuntos de datos se encuentran empaquetados en los siguientes enlaces directos del repositorio:

| Paquete | Contenido Principal | Enlace de Descarga |
| :--- | :--- | :--- |
| **📁 Datos del Proyecto** | Dataset procesado (`dataset_centinela.csv`), particiones por planta y recursos de entrada. | [⬇️ Descargar Datos (.zip)](./datos/datos_centinela.zip) |
| **📁 Documentación Técnica** | Propuesta de datos Fase 1 (PDF), informes metodológicos y anexos del proyecto. | [⬇️ Descargar Documentos (.zip)](./documentos/documentos_centinela.zip) |

> ⚠️ *Nota:* Asegúrate de mantener los archivos `.zip` dentro de las carpetas `./datos/` y `./documentos/` de tu repositorio (o actualiza la ruta en los enlaces de arriba según el nombre exacto de tus archivos).

---

## 🎯 1. Problema y Cliente
**Centinela del Café** es un sistema de alerta temprana diseñado para detectar hojas de café enfermas afectadas por **roya del cafeto** y **ácaro rojo**. 

* **El Cliente:** Pequeños caficultores.
* **Impacto del sistema:** La alerta temprana permite al agricultor tomar decisiones informadas sobre si debe inspeccionar detalladamente su cultivo o aplicar fungicida. Un sistema útil y preciso previene la pérdida de cosechas y evita gastos económicos e impactos ambientales innecesarios por aplicaciones a ciegas.

---

## ⚙️ 2. Arquitectura de Datos y Modalidades

### 🌐 Sistema Completo (Visión General)
El sistema definitivo integra dos modalidades de datos fusionadas en la **Fase 2**:
1. **Imagen:** Fotografías RGB de hojas de café.
2. **Serie temporal:** Variables climáticas diarias (temperatura, humedad, precipitación) de la zona del cultivo (Fuente: *NASA POWER*, mediante datos abiertos por coordenadas).

### 📐 Enfoque de la Fase 1 (Aplanamiento de Datos)
Para la primera fase, la modalidad de **Imagen se aplanó a una estructura tabular**:
* **Extracción de características:** De cada fotografía se calcularon **12 características de color**, correspondientes a la media y la desviación estándar de los canales **R, G, B** y **H, S, V**.
* **Etiqueta objetivo (Binaria):** 
  * `0`: Sana.
  * `1`: Enferma (Presencia de ácaro rojo o roya en niveles 1 a 4).

> 💡 *Nota de evolución:* En la Fase 2 este procesamiento tabular manual es reemplazado por una Red Neuronal Convolucional (CNN) operando directamente sobre la imagen cruda.

---

## 📊 3. Fuente de Datos y Originalidad

* **Dataset de Referencia:** Parraga-Alava, J., Cusme, K., Loor, A. y Santander, E. (2019). *RoCoLe: A robusta coffee leaf images dataset* (Versión 2) [Conjunto de datos]. Mendeley Data. [https://doi.org](https://doi.org)
* **Licencia:** CC BY 4.0 (Uso libre con atribución).
* **Volumen:** 1560 fotografías provenientes de 390 plantas de café robusta (4 fotos por planta), capturadas en Calceta, Manabí (Ecuador), con etiquetas estructuradas de estado y severidad.

### 🛡️ Justificación de Originalidad
Este proyecto **no es un reto ya resuelto** por las siguientes razones:
1. No proviene de plataformas comunes como Kaggle ni de sus réplicas. Es un conjunto de investigación formal indexado y publicado en *Data in Brief*.
2. El escenario del café figura explícitamente como una referencia admitida en la guía de la actividad.
3. **La versión tabular (promedios y desviaciones de color por foto) es una construcción propia del equipo.** No existe previamente como un producto público ni cuenta con una competencia o solución preexistente en la web.

---

## 🛠️ 4. Preparación de Datos y Cronograma

* **Análisis Exploratorio (EDA):** Evaluación rigurosa de la distribución de clases, escalas de las variables extraídas y verificación de datos faltantes.
* **Estrategia de Partición (70 / 15 / 15):** La división en conjuntos de Entrenamiento, Validación y Prueba se realiza estrictamente **por planta y no por foto**. Dado que las 4 fotos de una misma planta comparten idéntico fondo y condiciones de iluminación, mezclarlas al azar causaría una **fuga de datos** (*data leakage*), invalidando las métricas de rendimiento.
* **Escalado:** Cualquier ajuste o transformación de escala se calcula únicamente con el conjunto de entrenamiento para blindar la evaluación.

### 📅 Estado de Ejecución por Fases

| Fase | Descripción de la Modalidad | Componentes / Procesamiento | Estado |
| :--- | :--- | :--- | :--- |
| **Fase 1** | **Imagen Aplanada (Tabular)** | Extracción de 12 características de color (RGB / HSV) y entrenamiento del modelo base. | Completada ✅ |
| **Fase 2** | **Imagen Cruda + Serie Temporal** | Procesamiento por red neuronal convolucional (CNN) fusionada con datos climáticos agregados de *NASA POWER*. | Abordada 🚀 |

---

## ⚖️ 5. Riesgos Éticos, Limitaciones y Mitigación

* **Falsos Negativos (Roya/Ácaro no detectados):** Riesgo crítico que genera una falsa sensación de tranquilidad en el agricultor, provocando la pérdida potencial de su cosecha.
* **Falsos Positivos:** Conlleva a un gasto económico innecesario en insumos químicos y añade una carga contaminante evitable al ecosistema.
* **Sesgo de Cobertura (Limitación técnica):** Al usar datos de una sola región (Manabí), un único tipo de cultivo (Robusta) y condiciones fijas de cámara, la capacidad de generalización del modelo a otras geografías está restringida.
* **Alineación Imagen ↔ Clima (Fase 2):** El registro climático se integra de forma agregada por región y periodo temporal, no de manera individualizada por hoja (una hoja no posee su propio microclima). Esta limitación se declara de forma transparente desde el inicio del diseño.
* **Datos Personales:** El dataset es completamente anónimo; no se gestiona ninguna información sensible de individuos.
* **Marco Regulatorio:** El desarrollo se fundamenta bajo los principios del *Marco ético para la IA en Colombia* (Guío Español et al., 2021). El sistema se concibe estrictamente como una **herramienta de apoyo a la decisión humana**, garantizando que el control final permanezca siempre en manos del caficultor y no en una automatización autónoma.
