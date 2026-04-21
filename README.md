# 📊 Trabajo Práctico Integrador — Análisis de Datos 1B 2026

**FIUBA · CEIA · Grupo 5**

---

## 🧭 Índice

1. [Descripción general](#1-descripción-general)
2. [Dataset: Precios Claros – Base SEPA](#2-dataset-precios-claros--base-sepa)
3. [Problema de ML planteado](#3-problema-de-ml-planteado)
4. [Estructura del repositorio](#4-estructura-del-repositorio)
5. [⚠️ Git LFS — archivo de gran tamaño](#5-️-git-lfs--archivo-de-gran-tamaño)
6. [Requisitos e instalación](#6-requisitos-e-instalación)
7. [Pipeline de análisis y preparación de datos](#7-pipeline-de-análisis-y-preparación-de-datos)
8. [Detalle del notebook](#8-detalle-del-notebook)
9. [Resultados y conclusiones](#9-resultados-y-conclusiones)
10. [Evaluación y entrega](#10-evaluación-y-entrega)

---

## 1. Descripción general

Este repositorio contiene el **Trabajo Práctico Integrador** de la materia **Análisis de Datos 1B (2026)** del posgrado CEIA (Carrera de Especialización en Inteligencia Artificial) de la **FIUBA** (Facultad de Ingeniería, Universidad de Buenos Aires).

El objetivo del trabajo es **analizar y preparar** un dataset real para poder entrenar un modelo de Machine Learning supervisado. El foco está en todo el pipeline previo al entrenamiento: EDA, limpieza, feature engineering y reducción de dimensionalidad. El entrenamiento del modelo en sí **no es requerido**.

El grupo trabajó con la **Base SEPA (Sistema Electrónico de Publicidad de Precios Argentinos)** de la cadena **Vea (Cencosud)**.

---

## 2. Dataset: Precios Claros – Base SEPA

### ¿Qué es SEPA?

SEPA es el **Sistema Electrónico de Publicidad de Precios Argentinos**, un programa de la Secretaría de Comercio Interior de Argentina. Reúne diariamente los precios de comercios minoristas de más de **70.000 productos** en todo el país, lo que genera aproximadamente **1,4 millones de registros diarios**.

Para este trabajo se utilizó una snapshot de la cadena **Vea (Cencosud)**, que incluye datos de **~271 sucursales** distribuidas por todo el país.

### Archivos del dataset

| Archivo | Descripción | Tamaño |
|---|---|---|
| `productos_unificado.csv` | Precios de productos por sucursal (tabla principal) | ~180 MB (**LFS**) |
| `sucursales_unificado.csv` | Información de sucursales (nombre, tipo, coordenadas, etc.) | ~60 KB |
| `comercio_unificado.csv` | Información del comercio / cadena | ~250 B |

### Esquema de los datos

Los tres archivos se unen mediante las claves:

- `id_comercio`
- `id_bandera`
- `id_sucursal`

El dataset resultante post-join contiene columnas como:

| Columna | Tipo | Descripción |
|---|---|---|
| `productos_descripcion` | Texto | Nombre/descripción del producto |
| `productos_precio_lista` | Float | **Variable target** — precio de lista en pesos |
| `productos_precio_referencia` | Float | Precio de referencia (eliminado para evitar leakage) |
| `productos_cantidad_presentacion` | Float | Cantidad del producto en su presentación |
| `productos_unidad_medida_presentacion` | Texto | Unidad de medida (g, ml, un, etc.) |
| `productos_marca` | Texto | Marca del producto |
| `productos_cantidad_referencia` | Float | Cantidad de referencia |
| `productos_unidad_medida_referencia` | Texto | Unidad de medida de referencia |
| `sucursales_tipo` | Texto | Tipo de sucursal (Supermercado, Hipermercado, etc.) |
| `sucursales_latitud` / `sucursales_longitud` | Float | Coordenadas geográficas |
| `sucursales_provincia` | Texto | Provincia de la sucursal |

---

## 3. Problema de ML planteado

### Tipo: Regresión supervisada

**Objetivo:** Predecir el precio de lista (`productos_precio_lista`) de un producto en una sucursal dada.

**Variable target:** `productos_precio_lista` (variable continua, en pesos argentinos)

**Features considerados:**

| Feature | Naturaleza |
|---|---|
| `sucursales_tipo` | Categórica (Supermercado / Hipermercado / Autoservicio) |
| `provincia_nombre` | Categórica (enriquecida desde coordenadas) |
| `sucursales_latitud` / `sucursales_longitud` | Numérica continua |
| `productos_cantidad_presentacion` | Numérica continua |
| `productos_unidad_medida_presentacion` | Categórica |
| `productos_marca` | Categórica (alta cardinalidad) |
| `productos_cantidad_referencia` | Numérica continua |
| `productos_unidad_medida_referencia` | Categórica |

La hipótesis es que el precio de un producto está influenciado por su formato de presentación (tamaño/cantidad), la marca, el tipo y la ubicación geográfica de la sucursal.

---

## 4. Estructura del repositorio

```
CEIA_AdD25_grupo5/
│
├── tp (grupo 5).ipynb          # Notebook principal con todo el análisis
├── productos_unificado.csv     # Dataset de productos (Git LFS — ~180 MB)
├── sucursales_unificado.csv    # Dataset de sucursales
├── comercio_unificado.csv      # Dataset de comercios
├── requirements.txt            # Dependencias Python
├── .gitattributes              # Configuración de Git LFS (*.csv)
└── README.md                   # Este archivo
```

---

## 5. ⚠️ Git LFS — archivo de gran tamaño

Este repositorio usa **[Git Large File Storage (LFS)](https://git-lfs.com/)** para gestionar el archivo `productos_unificado.csv`, que pesa aproximadamente **180 MB** y supera el límite estándar de Git.

El archivo `.gitattributes` declara que **todos los archivos `.csv`** son gestionados por LFS:

```
*.csv filter=lfs diff=lfs merge=lfs -text
```

### ¿Cómo clonar correctamente este repositorio?

**Paso 1 — Instalar Git LFS** (si no lo tenés instalado):

```bash
# macOS (Homebrew)
brew install git-lfs

# Ubuntu / Debian
sudo apt-get install git-lfs

# Windows — descargá el instalador desde https://git-lfs.com o intenta
git lfs install
```

**Paso 2 — Inicializar Git LFS en tu sistema**:

```bash
git lfs install
```

**Paso 3 — Clonar el repositorio normalmente**:

```bash
git clone <url-del-repositorio>
```

Al clonar con LFS activo, el archivo `productos_unificado.csv` se descargará automáticamente con su contenido real.

> **⚠️ Advertencia:** Si clonás el repo sin Git LFS instalado, el archivo `productos_unificado.csv` tendrá solo un puntero de texto en lugar del CSV real, y el notebook fallará al intentar cargarlo.

---

## 6. Requisitos e instalación

### Dependencias

```txt
numpy>=1.21.0
pandas>=1.3.0
seaborn>=0.11.0
geopandas>=0.9.0
contextily>=1.2.0
matplotlib>=3.4.0
scipy>=1.7.0
scikit-learn>=0.24.0
tabulate>=0.10.0
```

### Instalación

```bash
# 1. Clonar el repositorio (ver sección de Git LFS arriba)
git clone <url-del-repositorio>
cd CEIA_AdD25_grupo5

# 2. Crear un entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Abrir el notebook
jupyter notebook "tp (grupo 5).ipynb"
```

> **Nota sobre `geopandas`:** En Windows puede requerir instalación manual de dependencias. Recomendamos usar conda en ese caso:
>
> ```bash
> conda install -c conda-forge geopandas contextily
> ```

---

## 7. Pipeline de análisis y preparación de datos

El notebook implementa un pipeline completo de 12 etapas:

```
Carga de datos
    │
    ▼
Join de los 3 CSV (productos + sucursales + comercio)
    │
    ▼
Enriquecimiento geográfico (provincia_nombre, provincia_region)
    │
    ▼
Análisis de valores faltantes (MCAR / MAR / MNAR)
    │
    ▼
EDA — Exploración y visualización
    │
    ▼
Errores, inconsistencias y outliers
    │
    ▼
Definición del problema de ML
    │
    ▼
Preprocesamiento (selección de columnas + imputación + split)
    │
    ▼
Feature Engineering (transformaciones + codificaciones + discretización)
    │
    ▼
Escalado (StandardScaler)
    │
    ▼
Selección de features (correlación + F-score + Mutual Information)
    │
    ▼
Evaluación de reducción de dimensionalidad (PCA)
    │
    ▼
Dataset final listo para modelado
```

---

## 8. Detalle del notebook

### Sección 0 — Importación de librerías

Importación de `numpy`, `pandas`, `seaborn`, `geopandas`, `contextily`, `matplotlib`, `scipy` y `sklearn`.

---

### Sección 1 — Carga y unión de datos

- Carga de los 3 archivos CSV.
- **Join** de `productos` con `sucursales` y `comercio` mediante `id_comercio`, `id_bandera` e `id_sucursal`.
- Dataset resultante: **~1.4 millones de filas**.
- Enriquecimiento geográfico: se derivan las features `provincia_nombre` y `provincia_region` a partir de las coordenadas de cada sucursal.
- **Prevención de Target Leakage:** se elimina `productos_precio_referencia`, que es una variable directamente derivada del target.

---

### Sección 2 — Análisis de valores faltantes

Los faltantes se clasifican según su mecanismo:

| Variable | Mecanismo | Justificación |
|---|---|---|
| `productos_precio_referencia` | MNAR | Sólo presente si el producto tiene precio de referencia distinto al de lista |
| Promociones (`productos_precio_promo_*`) | MNAR estructural | Sólo existen si el producto tiene promoción activa |
| Coordenadas de sucursales web | MAR | Las sucursales online no tienen ubicación física |
| `sucursales_barrio`, `sucursales_observaciones` | MNAR / MAR | Campos opcionales del formulario SEPA |

Se eliminan además las **features constantes** (varianza cero), que no aportan información al modelo.

---

### Sección 3 — Análisis exploratorio de datos (EDA)

- Estadísticas descriptivas generales del dataset.
- Análisis de distribuciones numéricas (histogramas + boxplots).
- Identificación de asimetría (skewness) en el target `productos_precio_lista`.
- **Mapa geográfico interactivo** de sucursales usando `geopandas` + `contextily` sobre OpenStreetMap, coloreado por tipo de sucursal.
- Análisis por tipo de sucursal, marca, provincia y unidad de medida.

---

### Sección 4 — Identificación y tratamiento de errores, outliers e inconsistencias

#### 4.1 Errores e inconsistencias

- Se detectan y eliminan registros con `productos_precio_lista <= 10` (errores de carga evidentes).
- Se verifican productos con `productos_cantidad_presentacion <= 0`.

#### 4.2 Outliers

- **Método IQR:** detección del rango aceptable para el precio de lista.
- **Winsorización (Capping):** los valores de precio son recortados al rango **[P1, P99]** para mitigar el efecto de valores extremos sin perder registros.

  ```
  Precio capped al rango [P1, P99]
  ```

- La distribución de precios es **fuertemente asimétrica y leptocúrtica**.

---

### Sección 5 — Definición del problema de ML supervisado

- **Tipo:** Regresión supervisada.
- **Target:** `productos_precio_lista` (precio continuo en pesos).
- **Hipótesis:** el precio es función del formato del producto, la marca y la ubicación/tipo de sucursal.

---

### Sección 6 — Preprocesamiento y limpieza

#### 6.1 Selección de columnas

Se seleccionan sólo las columnas relevantes para el modelo (ver tabla en §3).

#### 6.2 Imputación de faltantes

| Variable | Estrategia | Supuesto |
|---|---|---|
| `sucursales_latitud` / `sucursales_longitud` | Mediana a nivel provincia | MAR — la ausencia depende de si es sucursal web |
| `productos_marca` | Relleno con `'DESCONOCIDA'` | MNAR — el campo es opcional |
| `productos_cantidad_presentacion` | Mediana global | — |
| `provincia_nombre` | Relleno con `'DESCONOCIDA'` | — |
| Promociones | Excluidas del modelo | MNAR estructural |

Finalmente se eliminan las filas que aún contengan faltantes.

#### 6.3 Split train / test

El split se realiza **antes** de cualquier transformación que dependa de estadísticos (escalado, frequency encoding) para evitar **data leakage**.

---

### Sección 7 — Feature Engineering

#### 7.1 Nuevos features creados

| Feature nuevo | Descripción | Motivación |
|---|---|---|
| `log_cantidad_presentacion` | `log(productos_cantidad_presentacion)` | Reduce la asimetría de la variable; alta correlación con el precio |
| `es_ean` | Indicador binario: 1 si el código de barras es EAN estándar, 0 si es código interno | Los productos con EAN tienden a ser de marcas reconocidas con precios distintos |

#### 7.2 Codificación de variables categóricas

| Variable | Técnica | Justificación |
|---|---|---|
| `sucursales_tipo` | **Ordinal Encoding** | Los tipos de sucursal tienen un orden implícito de tamaño/precio |
| Tipos de sucursal (dummies) | **One-Hot Encoding** | Variables indicadoras para cada tipo |
| `productos_unidad_medida_presentacion` | **Label Encoding** | Cardinalidad baja, sin orden |
| `productos_marca` | **Frequency Encoding** (`marca_freq`) | Alta cardinalidad — se reemplaza la categoría por la frecuencia de aparición en train |
| `provincia_nombre` | **Frequency Encoding** (`provincia_freq`) | Alta cardinalidad — misma estrategia que marca |

#### 7.3 Discretización

- `sucursales_latitud` se discretiza en **5 bandas latitudinales** (norte / centro-norte / centro / centro-sur / sur) para capturar diferencias regionales de precio sin exponer la coordenada cruda al modelo.

---

### Sección 8 — Escalado y normalización

Se aplica **StandardScaler (z-score)** a las features numéricas:

```
X_train_scaled = scaler.fit_transform(X_train_num)   # ajuste solo sobre train
X_test_scaled  = scaler.transform(X_test_num)         # transformación sobre test
```

Esta estrategia evita **data leakage**: los estadísticos de normalización (media y desviación estándar) se calculan exclusivamente sobre el conjunto de train.

---

### Sección 9 — Correlación y selección de features

#### 9.1 Matriz de correlación

Heatmap de correlaciones entre todas las features numéricas y el target, calculado sobre el conjunto de train escalado.

#### 9.2 Selección de features (filtros)

Se aplican tres métodos de filtro:

| Método | Descripción |
|---|---|
| **Correlación de Pearson** | Relación lineal directa con el target |
| **F-score (`f_regression`)** con `SelectKBest` | Prueba estadística ANOVA de relación lineal |
| **Mutual Information** (`mutual_info_regression`) | Captura relaciones no lineales |

**Resultados clave:**

- `marca_freq`, `log_cantidad_presentacion` y `productos_cantidad_presentacion` muestran la **mayor relevancia** con el target.
- `tipo_Hipermercado`, `tipo_Supermercado` y `sucursales_tipo_ord` tienen una **ínfima relación** con el target y se eliminan del dataset.

---

### Sección 10 — Reducción de dimensionalidad (PCA)

Se evalúa PCA como posible técnica de extracción de features:

| Aspecto | Selección de features | Extracción (PCA) |
|---|---|---|
| En qué consiste | Conservar un subconjunto de variables originales | Crear nuevas variables combinando las originales |
| Cuándo aplicarlo | Dimensionalidad moderada | Dimensionalidad alta |
| Interpretabilidad | **Alta** | Baja |
| Técnicas | Correlación, ANOVA, Chi², Mutual Info | PCA, LDA, t-SNE, UMAP |

**Conclusión:** dado que el dataset final tiene **dimensión 8 (moderada)** y que se prioriza la interpretabilidad de los datos, se descarta PCA y se opta por técnicas de **selección** de features.

---

### Secciones 11–12 — Dataset final y conclusiones

El pipeline genera:

- `X_train_scaled` y `X_test_scaled`: conjuntos de features numéricos, escalados y sin valores faltantes, listos para alimentar un modelo de regresión.
- `y_train` y `y_test`: vectores del target `productos_precio_lista`.

---

## 9. Resultados y conclusiones

1. **Dataset SEPA:** ~1.4M de registros de precios de la cadena Vea (Cencosud) en ~271 sucursales distribuidas por todo el país.

2. **Calidad de datos:** Se identificaron dos tipos de faltantes:
   - **MNAR estructural:** campos opcionales del formulario SEPA (promociones, barrio, observaciones).
   - **MAR operacional:** coordenadas ausentes en sucursales web.

3. **Variables más informativas:** La cantidad de presentación (y su transformación logarítmica), la marca del producto (codificada por frecuencia) y la ubicación geográfica de la sucursal son los predictores más relevantes del precio de lista.

4. **Outliers:** La distribución de precios es fuertemente asimétrica (leptocúrtica). Se aplicó **winsorización** al rango [P1, P99] para mitigar el efecto de valores extremos sin eliminar registros.

5. **Feature engineering:** Se crearon features derivados (log-transform, indicadores binarios `es_ean`, frecuencias de marca/provincia, bandas latitudinales) que enriquecen la representación del problema.

6. **PCA descartado:** Por dimensionalidad moderada del dataset final (8 features) y preferencia por mantener la interpretabilidad, se eligió selección sobre extracción.

7. **Dataset final listo:** El pipeline genera conjuntos train/test con features numéricos, escalados y sin faltantes, listos para alimentar un modelo de regresión supervisada.

---

## 10. Evaluación y entrega

| Ítem | Detalle |
|---|---|
| **Materia** | Análisis de Datos 1B 2026 — FIUBA · CEIA |
| **Grupo** | Grupo 5 |
| **Dataset** | Precios Claros – Base SEPA (Cencosud / Vea) |
| **Entrega** | Presentación Google Slides (máx. 5 diapositivas) |
| **Fecha límite de entrega** | 21/04/2026 |
| **Exposición oral** | 22/04/2026 y 25/04/2026 (15 min por grupo: 10' exposición + 5' devolución) |

### Criterios de evaluación

- Entendimiento del dataset (características, desafíos).
- Elección y aplicación de técnicas de visualización.
- Justificación de las técnicas de preparación de datos.
- Profundidad de las conclusiones.
- Calidad de la presentación.

> *El entrenamiento de modelos no es requerido y no impacta en la nota.*

---

*TP realizado para Análisis de Datos 1B 2026 — FIUBA · CEIA — Grupo 5*
