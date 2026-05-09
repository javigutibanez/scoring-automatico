## Scoring Automático v2.0 — Semana 1 completada

### Qué hace esta semana
Introducción a scikit-learn y clasificación. Reformulación del Analizador de PYMEs
como un problema de Machine Learning — el modelo aprende automáticamente qué empresas
son buenas inversiones sin que definas los pesos manualmente.

### Pipeline de ML construido
1. Dataset preparado — 8 empresas, 6 features, variable objetivo `es_top`
2. Split train-test — 6 empresas train / 2 empresas test
3. Escalado con StandardScaler
4. Regresión Logística — baseline
5. Random Forest — segundo modelo
6. Cross-validation con StratifiedKFold (4 folds)

### Resultados
| Modelo | Accuracy (test) | F1 cross-val |
|---|---|---|
| Regresión Logística | 0.50 | ver notebook |
| Random Forest | 1.00 | ver notebook |

**Variable más importante:** `margen_bruto_pct` — confirma la intuición del Mes 2.
**Ganador de la semana:** Random Forest → pasa a competir con XGBoost en la Semana 2.

### Nota sobre el dataset pequeño
Con 9 empresas las métricas tienen alta varianza. Los resultados son orientativos —
la estructura del código es la misma que se usaría con 9.000 empresas.

### Archivos
- **scoring_automatico_v01.ipynb** — notebook completo de la semana

## Scoring Automático v3.0 — Semana 2 completada

### Qué hace esta semana
XGBoost como modelo central del proyecto — el algoritmo más usado en ML financiero.
Importancia de variables, SHAP values para explicabilidad, GridSearchCV para
optimización y modelo exportado con joblib listo para el Mes 6.

### Problema resuelto — dataset pequeño
Con 8 empresas y clases balanceadas, XGBoost no generaba splits con los parámetros
estándar (importancias a 0.0). Solución: `scale_pos_weight = n_neg/n_pos` y entrenar
sobre el dataset completo en lugar de train/test split.

### Resultados clave

**Importancia de variables — XGBoost**
`margen_bruto_pct` con importancia 1.0 — con `max_depth=1` el modelo solo puede
hacer un split y el margen bruto es la variable que mejor separa TOP de MID/LOW.
Confirma empíricamente la intuición del Mes 2.

**SHAP values**
Margen alto (rojo) → empuja hacia TOP.
Margen bajo (azul) → empuja hacia MID/LOW.
El resto de variables no influyen con un solo split.

**GridSearchCV — mejores hiperparámetros**
Ver output del notebook — 27 combinaciones probadas con cross-validation de 3 folds.

**Modelo exportado**
Listo para cargarse en el Mes 6 con una línea de código.

### Archivos
- **scoring_automatico_v03.ipynb** — notebook completo Semana 1 + Semana 2
- **importancia_variables_xgb_v3.png** — importancia de variables
- **shap_summary_v3.png** — gráfico SHAP beeswarm
- **scoring_ml_probabilidades_v3.png** — ranking de empresas por probabilidad
- **modelo_scoring_v3.joblib** — modelo XGBoost optimizado
- **features_scoring_v3.joblib** — lista de variables del modelo

### Lección clave
Con pocos datos XGBoost necesita `scale_pos_weight` para romper el empate entre
clases balanceadas. Con 50+ empresas del Registro Mercantil en el Mes 6, el modelo
usará todas las variables y las importancias serán más ricas.
