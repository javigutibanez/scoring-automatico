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

## Scoring Automático v4.0 — Semana 3 completada

### Qué hace esta semana
Feature engineering — construcción de 7 ratios financieros avanzados y 4 variables
de interacción, selección automática de las más informativas y modelo final entrenado
y comparado con la Semana 2.

### Bloque 11 — Ratios financieros avanzados
7 nuevas variables derivadas de los datos del Mes 2:

| Variable | Qué mide |
|---|---|
| coste_por_empleado | Eficiencia de costes por persona |
| activo_por_empleado | Intensidad de activos por persona |
| deuda_por_empleado | Carga de deuda por persona |
| roa_estimado | Return on Assets — rentabilidad sobre activos |
| roe_estimado | Return on Equity — rentabilidad sobre capital propio |
| deuda_ebitda | Años necesarios para pagar la deuda |
| años_pago_deuda | Mismo ratio — perspectiva temporal |

### Bloque 12 — Variables de interacción
4 variables que combinan ratios existentes:

| Variable | Qué captura |
|---|---|
| margen_x_crecimiento | Empresas rentables Y en crecimiento simultáneamente |
| solvencia_compuesta | Margen alto + deuda baja en un solo indicador |
| eficiencia_relativa | Facturación/empleado vs media del sector |
| tamano_solvencia | Empresas grandes con balance limpio |

### Bloque 13 — Selección de features
SelectKBest con F-score selecciona las features con mayor poder discriminante
entre TOP y MID/LOW. Top features seleccionadas → ver output del notebook.

### Bloque 14 — Modelo final
GridSearchCV sobre el dataset enriquecido. Comparativa directa con la Semana 2
usando el mismo dataset y los mismos folds.

### Archivos
- **scoring_automatico_v04.ipynb** — notebook completo de la semana
- **modelo_scoring_v4_final.joblib** — modelo XGBoost con features enriquecidas
- **scaler_scoring_v4_final.joblib** — scaler para las nuevas features
- **features_scoring_v4_final.joblib** — lista de top features seleccionadas

### Lección clave
Con 8 empresas el feature engineering no mejora dramáticamente el F1 — el dataset
es demasiado pequeño para que las nuevas variables alcancen significancia estadística.
Con 50+ empresas del Registro Mercantil en el Mes 6, el ROA, ROE y solvencia
compuesta serán variables altamente informativas.

## Scoring Automático v5.0 — Semana 4 completada · Mes 5 cerrado

### Qué hace esta semana
Semana de integración y cierre del Mes 5. Pipeline completo de scikit-learn,
scoring automático integrado sobre las 9 PYMEs, dashboard final y Excel ejecutivo.

### Bloque 15 — Pipeline completo
Pipeline scikit-learn que encadena escalado + XGBoost en un solo objeto.
Predicción en una línea — sin necesidad de escalar manualmente antes de predecir.

Demo con empresa nueva (margen 68%, deuda 0.15, crecimiento 5.3%):
**Probabilidad de ser TOP: 72.4% → Clasificación: TOP**

### Bloque 16 — Scoring integrado en el Analizador v0.5
Pipeline aplicado sobre las 9 empresas del dataset original.

| Empresa | Prob TOP | Clasificación | Score manual |
|---|---|---|---|
| Gestoría Pérez | 0.724 | TOP ✓ | 0.868 |
| Academia Idiomas Sol | 0.724 | TOP ✓ | 0.808 |
| Clínica Dental Martínez | 0.724 | TOP ✓ | 0.652 |
| Resto (6 empresas) | 0.276 | MID/LOW | — |

**Correlación ML vs scoring manual: 0.76** — alta coincidencia entre ambos modelos.

### Bloque 17 — Dashboard final y Excel
Dashboard de 4 paneles: ranking ML, ML vs manual, importancia de variables
y tabla resumen del Mes 5.

Excel con 4 hojas: Ranking ML, Importancia variables, ML vs Manual,
Resumen ejecutivo.

### Archivos
- **scoring_automatico_v05.ipynb** — notebook completo del Mes 5
- **pipeline_scoring_v5.joblib** — pipeline listo para producción en el Mes 6
- **analizador_v5_scoring_ml.png** — ranking de las 9 empresas por probabilidad
- **dashboard_mes5_final.png** — dashboard completo del Mes 5
- **scoring_automatico_v5_final.xlsx** — Excel con 4 hojas

### Resumen del Mes 5
| Métrica | Resultado |
|---|---|
| Features creadas | 13 (6 originales + 7 nuevas) |
| Features seleccionadas | 6 por F-score |
| F1 cross-validation | 0.333 |
| Correlación ML vs manual | 0.76 |
| Empresas TOP identificadas | 3 (consistentes con scoring manual) |
| Pipeline exportado | pipeline_scoring_v5.joblib |


