# Clasificación del Riesgo de Inundación por Parroquia
## Provincia de Los Ríos, Ecuador

Aplicación web (Flask + Leaflet) que muestra un mapa interactivo con la categoría de riesgo de inundación estimada por un modelo de Machine Learning para cada parroquia de la provincia de Los Ríos, Ecuador.

**Proyecto del Segundo Parcial — Ciencia de Datos e Inteligencia Artificial**

---

## URL pública
**https://andresvl.pythonanywhere.com/**

## Notebook interactivo
**https://colab.research.google.com/drive/1Tgg5C4jFLlEsXRzts72ux0JwvCpzs0b6**

---

## Estructura del repositorio

```
proyecto-riesgo-inundacion/
├── app.py                          # Backend Flask
├── requirements.txt                # Dependencias del proyecto
├── README.md                       # Este archivo
├── modelo.pkl                      # Modelo entrenado (Regresión Logística)
├── scaler.pkl                      # Preprocesador de variables numéricas
├── label_encoder.pkl               # Decodificador de etiquetas de riesgo
├── los_rios.geojson                # Polígonos de las 30 parroquias (INEC/ArcGIS)
├── predicciones_parroquias.csv     # Predicciones del modelo por parroquia
└── notebook/
    └── Proyecto_LosRios_FINAL.ipynb  # Notebook técnico completo (EDA + Modelado)
```

---

## Cómo funciona

`app.py` combina el GeoJSON con el CSV de predicciones, emparejando por código de parroquia (`DPA_PARROQ` — `codigo_parroquia`).

El mapa interactivo permite:
- **Hover (pasar el cursor):** muestra nombre de la parroquia, cantón y provincia
- **Clic (popup):** muestra la categoría de riesgo (bajo / medio / alto) y el score del modelo
- **Colores por nivel:** verde = bajo, naranja = medio, rojo = alto
- **Leyenda** explicando la simbología

Las predicciones fueron generadas con **Regresión Logística**, el modelo con mejor Recall macro (0.77) y AUC-ROC (0.95) entre los cuatro evaluados. `app.py` solo lee el CSV ya calculado — no ejecuta el modelo en cada petición.

---

## Modelos evaluados

| Modelo | Precisión | Recall | F1-score | AUC-ROC |
|---|---|---|---|---|
| **Regresión Logística ★** | 0.77 | **0.77** | 0.77 | 0.95 |
| Árbol de Decisión | 0.57 | 0.67 | 0.61 | 0.72 |
| Ensamble (RL+DT+RF) | 0.57 | 0.67 | 0.61 | 0.95 |
| RF Optimizado (GridSearch) | 0.57 | 0.67 | 0.61 | 0.90 |

★ Modelo seleccionado — mayor Recall macro. En gestión de riesgo, el Recall es la métrica prioritaria porque un falso negativo (clasificar una parroquia de alto riesgo como bajo riesgo) implica no activar medidas de protección.

---

## Ejecutar en local

```bash
pip install -r requirements.txt
python app.py
```

Abrir `http://127.0.0.1:5000` en el navegador.

---

## Despliegue

Alojado en **PythonAnywhere** (plan gratuito). El backend está en Flask y las visualizaciones geográficas en Leaflet, cargando `los_rios.geojson` y `predicciones_parroquias.csv` directamente desde el repositorio.

---

## Variables del modelo

| Variable | Tipo | Fuente |
|---|---|---|
| `precipitacion_mm` | Predictora | Open-Meteo Archive API (2019–2023) |
| `altitud_m` | Predictora | Open-Meteo Elevation API |
| `pendiente_idx` | Derivada | Calculada desde altitud |
| `distancia_rio_km` | Predictora | HydroSHEDS / Haversine |
| `densidad_pob` | Predictora | INEC Censo 2022 |
| `porc_urbano` | Predictora | INEC Censo 2022 / REDATAM |
| `ratio_lluviosa_seca` | Derivada temporal | Open-Meteo serie diaria |
| `riesgo` | **Objetivo** | Construida por criterio multicriterio |

---

## Integrantes — Grupo D

| Nombre |
|---|
| Parrales Moreno Kathia |
| Barros Rubio Sharick |
| Cruz Huacon Anthony |
| Velez Landaverea Andres |
| Iniguez Ruiz Juan |
| Zavala Betancourt Jeanpaul |

**Docente:** Ing. Darwin Rodríguez Merchan, MSc.  
**Universidad de Guayaquil** | Facultad de Ciencias Matemáticas y Físicas  
**Carrera:** Ciencia de Datos e Inteligencia Artificial  
**Curso:** CDDEIA-ELMA-4-1 | 2026
