# telecom_x2
Este repositorio contiene el código y los análisis para el desarrollo de un modelo predictivo de Machine Learning. El objetivo es identificar a los clientes con mayor probabilidad de cancelar sus servicios en Telecom X, permitiendo a la empresa anticiparse al problema.

# 📱 Telecom X - Predicción de Cancelación de Clientes (Churn) - Parte 2

## 📝 Propósito del Análisis

El objetivo principal de este proyecto es **desarrollar modelos predictivos de Machine Learning** capaces de identificar clientes con alta probabilidad de cancelar sus servicios en Telecom X. La empresa busca anticiparse al problema de la cancelación (churn) mediante la construcción de un pipeline robusto que incluye:

- Preprocesamiento y transformación de datos
- Análisis exploratorio y visualización
- Entrenamiento y evaluación de múltiples modelos
- Interpretación de resultados y generación de insights estratégicos

## 🔧 Proceso de Preparación de Datos

### Clasificación de Variables

| Tipo | Variables |
|------|-----------|
| **Numéricas** | `tenure`, `Charges.Monthly`, `Charges.Total`, `SeniorCitizen` |
| **Categóricas Binarias** | `gender`, `Partner`, `Dependents`, `PhoneService`, `PaperlessBilling` |
| **Categóricas Múltiples** | `InternetService`, `Contract`, `PaymentMethod`, `MultipleLines`, `OnlineSecurity`, `TechSupport`, `StreamingTV`, `StreamingMovies` |

### Transformaciones Aplicadas

- **Eliminación de columnas irrelevantes**: Se eliminó `customerID` por ser un identificador único sin valor predictivo.
- **Codificación de variables**:
  - Variables binarias (`Yes`/`No`, `Female`/`Male`) → mapeo a 1/0
  - Variables con 3+ categorías → One-Hot Encoding con `drop_first=True`
- **Manejo de valores nulos**: Se identificaron valores vacíos en `Churn` (eliminados) y espacios en `Charges.Total` (convertidos a 0).
- **Normalización**: Se aplicó `StandardScaler` a las variables numéricas para los modelos sensibles a escala.

### Separación de Datos

- **80% entrenamiento**, **20% prueba**
- **Estratificación** para mantener la misma proporción de cancelación en ambos conjuntos

---

## 🤖 Modelos Evaluados

Se entrenaron y compararon **5 modelos de clasificación**:

| Modelo | Tipo | Requiere Normalización |
|--------|------|------------------------|
| **Regresión Logística** | Sensible a escala | ✅ Sí |
| **K-Nearest Neighbors** | Sensible a escala | ✅ Sí |
| **Árbol de Decisión** | No sensible a escala | ❌ No |
| **Random Forest** | No sensible a escala | ❌ No |
| **XGBoost** | No sensible a escala | ❌ No |

---

## 📊 Resultados y Métricas

### Comparación de Modelos

| Modelo | Accuracy | Precision | Recall | F1-Score | AUC-ROC |
|--------|----------|-----------|--------|----------|---------|
| **Regresión Logística** | 0.729 | 0.493 | **0.759** | **0.598** | 0.822 |
| Árbol de Decisión | 0.725 | 0.489 | 0.757 | 0.594 | 0.819 |
| Random Forest | 0.762 | **0.544** | 0.647 | 0.591 | 0.821 |
| XGBoost | 0.745 | 0.515 | 0.676 | 0.585 | 0.799 |
| KNN | 0.755 | 0.544 | 0.476 | 0.508 | 0.757 |

### Mejor Modelo: **Regresión Logística**

- **Recall: 75.9%** → Detecta 3 de cada 4 clientes que cancelarán
- **Precision: 49.3%** → Cuando alerta, acierta 1 de cada 2 veces
- **F1-Score: 0.598** → Mejor equilibrio global

---

## 🔍 Factores Críticos de Cancelación

### 📈 Variables que AUMENTAN la cancelación

| Variable | Coeficiente | Odds Ratio | Interpretación |
|----------|-------------|------------|----------------|
| `Charges.Monthly` | +0.873 | 2.39x | Clientes con mayor cargo mensual tienen **139% más riesgo** |
| `Charges.Total` | +0.286 | 1.33x | Mayor gasto acumulado = 33% más riesgo |
| `PaperlessBilling` | +0.262 | 1.30x | Factura digital = 30% más riesgo |
| `SeniorCitizen` | +0.200 | 1.22x | Adultos mayores = 22% más riesgo |

### 📉 Variables que DISMINUYEN la cancelación

| Variable | Coeficiente | Odds Ratio | Interpretación |
|----------|-------------|------------|----------------|
| `tenure` | -1.575 | 0.21x | Clientes antiguos tienen **79% MENOS riesgo** |
| `PhoneService` | -0.241 | 0.79x | Tener teléfono reduce riesgo 21% |
| `Dependents` | -0.142 | 0.87x | Con dependientes = 13% menos riesgo |
| `Partner` | -0.015 | 0.99x | Con pareja = 1% menos riesgo |

### Hallazgos Clave

1. **Antigüedad es el factor #1**: Clientes nuevos (<6 meses) son los más vulnerables
2. **Precio mensual importa**: Cargos mensuales altos = alto riesgo
3. **Servicios básicos protegen**: Teléfono, dependientes y pareja reducen riesgo
4. **Factura digital = alerta**: Clientes sin factura física tienen menos compromiso

---

## 🚀 Estrategias de Retención Recomendadas

| Estrategia | Acción Concreta | Impacto Esperado |
|------------|-----------------|------------------|
| **Onboarding primeros 6 meses** | Programa de bienvenida con seguimiento mensual | Reducir churn 30% en clientes nuevos |
| **Revisión de precios** | Analizar competitividad de cargos mensuales | Retener clientes sensibles al precio |
| **Incentivar servicios básicos** | Bundles de PhoneService + Dependents | Aumentar permanencia 20% |
| **Campaña adultos mayores** | Ofertas especiales para SeniorCitizen | Reducir churn 15% en este segmento |

**Impacto estimado total**: Reducción del **25-30%** en la tasa de cancelación.

---

## ⚙️ Instrucciones de Ejecución

### Requisitos

Python 3.8+ y las siguientes bibliotecas: pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- xgboost
- jupyter


### Pasos para Ejecutar

1. **Clonar el repositorio**
2. **Instalar dependencias**
   ```bash
   pip install -r requirements.txt
3. **Ejecutar el cuaderno Jupyter**
     ``` bash
    jupyter notebook notebooks/telecomX.ipynb    
##  📌 Conclusiones Finales
El modelo de Regresión Logística demostró ser el más efectivo para predecir cancelación, con un Recall del 75.9%. Los principales factores de riesgo son cargos mensuales altos y baja antigüedad, mientras que contar con servicios básicos y responsabilidades familiares protege contra la cancelación. Se recomienda implementar el modelo en producción junto con las estrategias de retención propuestas.