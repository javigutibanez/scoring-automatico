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

