# ☕ Proyecto Centinela: Sistema de Alerta Temprana en Caficultura

**Propuesta de Datos — Fase 1 (Módulo 1)**  
*Maestría en Ciencia de Datos | Asignatura: Deep Learning*  
*Universidad Santo Tomás*

---

## 👥 Equipo de Trabajo
* **Andrés Julián Alba Gualteros**
* **Angie Camila Rojas**
* **Andrea Olarte Trujillo**

📅 **Fecha:** 30 de octubre de 2026  
📌 **Estado:** Fase 1 Activa 🚀

---

## 🎯 1. Problema y Cliente
**Centinela** es un sistema de alerta temprana diseñado para detectar hojas de café afectadas por **roya del cafeto** (*Hemileia vastatrix*) y **ácaro rojo** (*Oligonychus yothersi*).

* **Cliente objetivo:** Pequeños caficultores.
* **Propuesta de valor e impacto:** La detección oportuna permite al agricultor decidir si debe inspeccionar a fondo su lote o aplicar tratamientos focalizados. Esto reduce pérdidas de cosecha, optimiza costos en insumos agrícolas y mitiga el impacto ambiental derivado de fumigaciones preventivas a ciegas.

---

## ⚙️ 2. Arquitectura de Datos y Modalidades

### 🌐 Visión General del Sistema (Multimodal)
El sistema definitivo integrará dos fuentes de información que se fusionarán en la **Fase 2**:
1. **Visión por Computador (Imagen):** Fotografías RGB de hojas de café.
2. **Series Temporales (Clima):** Variables meteorológicas diarias de la zona del cultivo (temperatura, humedad relativa y precipitación), obtenidas mediante la API abierta *NASA POWER* por coordenadas geográficas (a confirmar en Fase 2).

### 📐 Enfoque de la Fase 1 (Representación Tabular)
Para esta primera etapa, la modalidad de **imagen se aplana hacia una estructura tabular** mediante ingeniería de características manual:
* **Variables predictoras (12 características):** Media (μ) y desviación estándar (σ) de los canales de color **R, G, B** y **H, S, V** extraídas de cada fotografía.
* **Variable objetivo (Clasificación binaria):**
  * `0`: Hoja sana.
  * `1`: Hoja enferma (presencia de ácaro rojo o roya en niveles de severidad 1 a 4).

> 💡 *Evolución técnica:* En la Fase 2, esta extracción manual será reemplazada por una Red Neuronal Convolucional (CNN) que operará directamente sobre las imágenes crudas.

---

## 📊 3. Fuente de Datos y Originalidad

* **Dataset base:** Parraga-Alava, J., Cusme, K., Loor, A. y Santander, E. (2019). *RoCoLe: A robusta coffee leaf images dataset* (Versión 2). Mendeley Data. [https://doi.org/10.17632/c5yvn32dzg.2](https://doi.org/10.17632/c5yvn32dzg.2)
* **Licencia:** CC BY 4.0 (Uso libre con atribución).
* **Volumen y origen:** 1.560 fotografías provenientes de 390 plantas de café robusta (4 capturas por planta), recolectadas en Calceta, Manabí (Ecuador), con etiquetas clínicas de estado y severidad.

### 🛡️ Justificación de Originalidad
Este desarrollo **no corresponde a un problema estándar ya resuelto** por tres razones clave:
1. **Fuente académica rigurosa:** No proviene de repositorios de competencias como Kaggle, sino de un conjunto de investigación validado y publicado en *Data in Brief*.
2. **Pertinencia temática:** El dominio caficultor está contemplado explícitamente como escenario válido dentro de los lineamientos de la asignatura.
3. **Construcción propia del dataset tabular:** La transformación a métricas estadísticas de color (RGB/HSV) por imagen constituye un producto de ingeniería inédito generado por el equipo.

---

## 🛠️ 4. Preparación de Datos y Plan de Trabajo

* **Análisis Exploratorio (EDA):** Evaluación del balance de clases, dispersión de las variables de color y verificación de integridad (ausencia de valores nulos o corruptos).
* **Partición libre de fuga de datos (70 / 15 / 15):** La división en conjuntos de Entrenamiento (*Train*), Validación (*Val*) y Prueba (*Test*) se ejecutará **agrupada por identificador de planta (`plant_id`) y no por fotografía individual**. Dado que las 4 fotos de una misma planta comparten fondo e iluminación, una partición aleatoria introduciría *data leakage* y sobreestimaría las métricas.
* **Preprocesamiento y escalado:** Cualquier normalización o estandarización se ajustará (`fit`) exclusivamente sobre el conjunto de entrenamiento antes de transformarse en validación y prueba.

### 📅 Cronograma por Fases

| Fase | Modalidad de Datos | Arquitectura / Procesamiento | Estado |
| :--- | :--- | :--- | :--- |
| **Fase 1** | **Imagen Aplanada (Tabular)** | Extracción de 12 variables de color (RGB / HSV) y entrenamiento de modelos base. | Activa 🚀 |
| **Fase 2** | **Imagen Cruda + Serie Temporal** | Red Neuronal Convolucional (CNN) fusionada con datos climáticos agregados (*NASA POWER*). | Planificada ⏳ |

---

## 📂 5. Entregables y Estructura del Repositorio

| Recurso | Descripción | Acceso |
| :--- | :--- | :--- |
| **Propuesta Técnica (PDF)** | Documento formal con el planteamiento, alcance y metodología de la Fase 1. | [📄 Ver PDF](./documentos/Propuesta_de_Datos_Centinela_Fase1.pdf) |
| **Dataset Procesado (CSV)** | Matriz tabular con las 12 características de color calculadas y etiquetas binarias. | [📊 Descargar CSV](./documentos/dataset_centinela.csv) |

```text
Proyecto-centinela/
├── documentos/
│   ├── Propuesta_de_Datos_Centinela_Fase1.pdf
│   └── dataset_centinela.csv
└── README.md
```

---

## ⚖️ 6. Riesgos Éticos, Limitaciones y Mitigación

* **Falsos Negativos (Riesgo crítico):** No detectar roya o ácaro rojo puede generar una falsa seguridad en el caficultor y derivar en pérdidas severas de producción. Se priorizará el monitoreo de métricas sensibles a esta clase (como *Recall* / Sensibilidad).
* **Falsos Positivos:** Alertas erróneas implican gastos innecesarios en fungicidas/acaricidas y contaminación ambiental evitable.
* **Sesgo de cobertura geográfica y varietal:** Al entrenarse con café *Robusta* bajo condiciones controladas de cámara en Manabí (Ecuador), la generalización directa a variedades como *Arábica* o a distintas condiciones lumínicas en campo requiere calibración futura.
* **Granularidad climática (Fase 2):** Las variables meteorológicas se asociarán a nivel regional y temporal, no como microclima individual por hoja. Esta limitación metodológica se documenta de forma transparente.
* **Privacidad de datos:** El conjunto de datos contiene exclusivamente registros botánicos y ambientales; no involucra datos personales ni información sensible.
* **Marco ético y gobernanza:** El diseño se alinea con el *Marco Ético para la Inteligencia Artificial en Colombia* (Guío Español et al., 2021). **Centinela** opera estrictamente como un sistema de apoyo a la decisión (*Human-in-the-Loop*), manteniendo el criterio agronómico y la decisión final en manos del caficultor.
