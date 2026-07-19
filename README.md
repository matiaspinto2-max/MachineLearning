[README (1).md](https://github.com/user-attachments/files/30170816/README.1.md)
# Réplica y extensión: Chen (2023) — Predicción del precio de Bitcoin con Machine Learning

Trabajo de portafolio semestral de la asignatura **Ciencia de Datos para la Economía**,
Ingeniería Comercial, Universidad Diego Portales.

**Profesor:** Luis Cuevas Parra
**Grupo:** Matias Pinto Gallardo

---

## 1. Descripción del proyecto

Este proyecto replica y extiende el paper:

> Chen, J. (2023). *Analysis of Bitcoin price prediction using machine learning*.
> Journal of Risk and Financial Management, 16(1), 51.
> https://doi.org/10.3390/jrfm16010051

Los autores comparan **Random Forest Regression** y una red **LSTM** para predecir el
precio de cierre diario de Bitcoin, evaluando el desempeño con RMSE, MAPE y Directional
Accuracy (DA), y contrastando la diferencia entre modelos con el test de Diebold-Mariano.

En este trabajo:

1. Replicamos los datos, el preprocesamiento y los dos modelos del paper original.
2. Proponemos e implementamos un modelo adicional no utilizado por los autores:
   **Gradient Boosting Regressor**.
3. Comparamos el desempeño de los tres modelos con las mismas métricas del paper, más
   el test de Clark-West contra un benchmark de caminata aleatoria.
4. Concluimos cuál modelo resulta más adecuado para este problema, contrastando con lo
   reportado por los autores.

## 2. Estructura del repositorio

```
├── Bitcoin_Chen_Replica_Corregido.ipynb   # Cuaderno principal (ejecutable de principio a fin)
├── scripts/
│   └── descarga_datos.py                  # Script independiente de obtención de datos
├── requirements.txt                       # Dependencias del proyecto
└── README.md                              # Este archivo
```

## 3. Datos

**Fuente:** Yahoo Finance, vía el paquete [`yfinance`](https://pypi.org/project/yfinance/).

**Variables:** precio diario de Bitcoin (Open, High, Low, Close, Volume) más 23 series
de mercado relacionadas: otras criptomonedas (ETH, LTC, XRP, DASH, DOGE), commodities
(oro, plata, cobre, petróleo), índices bursátiles (S&P 500, DJI, NASDAQ, Nikkei 225,
CSI 300), tipos de cambio (EUR, GBP, JPY, CAD, AUD, SGD, CNY, RUB, DXY) y la tasa del
Tesoro estadounidense a 10 años.

**Periodo:** 31-03-2015 a 01-04-2022, replicando la muestra original del paper.

Los datos se descargan automáticamente al ejecutar el notebook o el script
`scripts/descarga_datos.py`; no requieren descarga manual ni claves de API.

## 4. Instrucciones de ejecución

### 4.1 Clonar el repositorio

```bash
git clone <URL-del-repositorio>
cd <nombre-del-repositorio>
```

### 4.2 Crear un entorno virtual e instalar dependencias

```bash
python -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 4.3 Opción A — Ejecutar el notebook completo (recomendado)

Abrir `Bitcoin_Chen_Replica_Corregido.ipynb` en Jupyter, Google Colab o VS Code, y
ejecutar todas las celdas en orden ("Run All"). El notebook descarga los datos,
realiza el preprocesamiento, entrena los tres modelos y genera todas las tablas y
gráficos de resultados.

### 4.4 Opción B — Descargar los datos por separado

```bash
python scripts/descarga_datos.py
```

Esto genera `data/btc_dataset_raw.csv` con los datos crudos, útil si se quiere
inspeccionar el dataset antes de correr el notebook completo.

## 5. Modelos implementados

| Modelo | Origen | Descripción |
|---|---|---|
| Random Forest Regression | Paper original | 500 árboles, profundidad máxima 10 |
| LSTM | Paper original | Red recurrente apilada de 4 capas con dropout creciente |
| Gradient Boosting Regressor | Propuesto por el grupo | 300 estimadores, `loss='huber'` para robustez a outliers |

## 6. Métricas de evaluación

- **RMSE** (Root Mean Squared Error)
- **MAPE** (Mean Absolute Percentage Error)
- **DA** (Directional Accuracy): porcentaje de aciertos en la dirección del cambio de precio
- **Test de Diebold-Mariano**: significancia de la diferencia de desempeño entre pares de modelos
- **Test de Clark-West**: significancia de cada modelo frente a un benchmark de caminata aleatoria

## 7. Resultados y conclusiones

Ver la Sección 9 ("Discusión y conclusiones") del notebook para la tabla comparativa
final, la discusión sobre si los resultados replican cualitativamente al paper original
y las limitaciones de la réplica.

## 8. Limitaciones

- No se incluyen variables on-chain (hash rate, dificultad de minería) ni de atención
  pública (Google Trends), presentes en la literatura sobre predicción de Bitcoin pero
  no disponibles vía `yfinance`.
- La red LSTM se entrena con un número acotado de épocas y sin búsqueda exhaustiva de
  hiperparámetros, por restricciones de tiempo y cómputo.
- Al usar datos actuales de Yahoo Finance en lugar del repositorio original de los
  autores, pueden existir pequeñas diferencias respecto a una réplica exacta.

## 9. Referencias

Chen, J. (2023). Analysis of Bitcoin price prediction using machine learning. *Journal
of Risk and Financial Management*, 16(1), 51. https://doi.org/10.3390/jrfm16010051
