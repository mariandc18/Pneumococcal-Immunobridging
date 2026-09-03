# Estudio de Inmunopuente de Quimi-Vio® 11

Análisis estadístico de los datos de inmunogenicidad del ensayo clínico aleatorizado del candidato vacunal **Quimi-Vio® 11**, desarrollado en colaboración con el **Instituto Finlay de Vacunas**. Forma parte del proyecto de tesis de licenciatura en Ciencia de Datos de la Facultad de Matemática y Computación, Universidad de La Habana.

---

## 📋 Descripción

El notebook implementa un pipeline completo de análisis estadístico sobre los títulos de anticuerpos **OPA (Actividad Opsonofagocítica)** para 9 serotipos neumocócicos, en dos poblaciones: niños y adultos.

El objetivo es **predecir la eficacia de Quimi-Vio® 11 en adultos** mediante un análisis de **inmunopuente** (*immunobridging*): si la respuesta inmune en adultos es no inferior a la de niños —población en la que ya se demostró eficacia clínica—, se puede inferir que la vacuna también será eficaz en adultos. La respuesta de anticuerpos OPA actúa como **correlato de protección**.

---

## Serotipos analizados

`Sp1` · `Sp5` · `Sp6A` · `Sp6B` · `Sp14` · `Sp18C` · `Sp19A` · `Sp19F` · `Sp23F`

---

## Pipeline de análisis

### 1. Ingesta y preprocesamiento
- Extracción de tablas del PDF del ensayo clínico con `camelot`
- Limpieza de datos: eliminación de filas con valores inválidos y columnas vacías.
- Transformación logarítmica de los títulos OPA para normalizar la distribución

### 2. Análisis exploratorio (EDA)
- Histogramas y boxplots en escala original y logarítmica
- Análisis del porcentaje de valores por debajo del **LLOQ** (Lower Limit of Quantification, `log(4)`) por serotipo
- Curvas **RCDC** (Reverse Cumulative Distribution Curves) comparativas entre niños y adultos

### 3. Supuestos estadísticos
- **Test de Levene** (centrado en la mediana) para homocedasticidad entre grupos
- **QQ plots** con tres pruebas de normalidad simultáneas por serotipo:
  - Shapiro-Wilk
  - D'Agostino-Pearson
  - Anderson-Darling

### 4. Estimación del GMT con datos censurados

Los datos censurados (valores bajo el LLOQ) se manejan con dos métodos:

| Método | Descripción |
|---|---|
| **Sustitución** | Reemplaza los valores censurados por `LLOQ / 2` |
| **Maximum Likelihood Estimation** | Estima el GMT asumiendo distribución log-normal truncada, sin descartar ni imputar los valores censurados |

Para cada método se calculan:
- **GMT** (Geometric Mean Titer) con intervalo de confianza del 95%
- **GMR** (Geometric Mean Ratio = Adultos / Niños) con IC 95%

### 5. Análisis de no inferioridad (inmunopuente)

Se evalúa si la respuesta inmune en adultos es **no inferior** a la de niños bajo el criterio:

> *El límite inferior del IC 95% del GMR debe ser mayor que 0.5*
---

## ⚙️ Requisitos

```bash
pip install -r requirements.txt
```
