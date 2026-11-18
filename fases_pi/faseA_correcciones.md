# Fase A: correcciones del notebook de Semana 1

Notebook: `notebooks/Ejercicio_Practico_Semana1.ipynb` (respaldo previo: `notebooks/Ejercicio_Practico_Semana1_original.ipynb`).
Ejecución: Restart & Run All con `.venv` (Python 3.14.4, scikit-learn 1.9.1), SEMILLA=42. 15 celdas de código, `execution_count` del 1 al 15, sin errores. Formato JSON conservado (indent 1, CRLF, sin escapar no-ASCII): al cargar y reescribir se obtiene el archivo byte a byte.
Las celdas se numeran por su índice 0-based en `notebook['cells']`.

| Comentario / regla | Estado | Qué cambió (celda) | Cifra antes | Cifra después |
|---|---|---|---|---|
| Canvas: random_state fijo en mutual_info | Ya cumplido | Nada. `mi_clf = partial(mutual_info_classif, random_state=SEMILLA)` (celda 16), usado en todos los SelectKBest (celdas 16 y 23) | - | - |
| Canvas: random_state fijo en TargetEncoder | Ya cumplido | Nada. `TargetEncoder(cv=StratifiedKFold(n_splits=5, shuffle=True, random_state=SEMILLA))` (celda 16); en sklearn 1.9 el `random_state` propio del encoder está en desuso | - | - |
| Canvas: test evaluado una sola vez | Ya cumplido | Solo la celda 27 predice sobre test (la celda 7 solo limpia y cuenta errores). Lo agregado en la celda 27 reutiliza las mismas predicciones `y_pred_test`, sin reajustar | F1 test 0.9593 | F1 test 0.9593 (igual) |
| Canvas: la guía dice que la fuga sube el F1 y el notebook muestra lo contrario | Ya cumplido (coherente) | Nada. Celda 16 + conclusión celda 17; la guía (Tabla 6 y texto siguiente) dice lo mismo: con k=6 no hay efecto sistemático; con k=3, ruido y duplicados sí infla | k=6 SVM +0.0011 (semilla 42), -0.0027 ± 0.0053 (20 particiones); k=3 +0.0503 ± 0.0260; ruido +0.0761 ± 0.0168; duplicados +0.4491 | Igual |
| R2: filas compartidas train vs val/test (merge sin User_ID) | Corregido (faltaba test) | Celda 27: merge de test con train por las 8 predictoras sin User_ID; F1 y exactitud separados en filas vistas y nuevas. Celda 28 actualizada (antes decía "no se verificó") | Train 200 únicas de 924; val 88 de 132 compartidas; test sin medir | Test: 93 de 101 compartidas, 8 nuevas; F1 macro 1.0000 en vistas y 0.4111 en nuevas |
| R2: F1 sobre filas no vistas o StratifiedGroupKFold | Ya cumplido | CV con `StratifiedGroupKFold` agrupando copias (celdas 16, 19-23, 26, 30); val nuevas en celda 31 | Val nuevas F1 0.2981 (n=44) | Igual; se suma test nuevas 0.4111 (n=8) |
| R3: GridSearchCV y Optuna con validación honesta | Ya cumplido | Nada en el código. Celda 26: ambos con `cv=StratifiedGroupKFold`, `groups=GRUPOS`, mismo espacio y 20 evaluaciones; salidas impresas. Solo cambian los tiempos de la nueva corrida (actualizados en la celda 28) | Grid 0.7826 en 1.22 s; Optuna 0.7826 en 2.91 s | Grid 0.7826 en 1.37 s; Optuna 0.7826 en 3.10 s (mismos parámetros) |
| R4: Wilson 95 % si n < 100 | Corregido | Función `wilson()` en celda 7; IC en % negativo por grupo DBSCAN (celda 19), exactitud y recall por clase en test (27), exactitud y recall por Gender en val (31). Conclusiones 21, 28, 32, 35 y checklist 43 actualizadas | Sin intervalos | Test nuevas 4/8, IC [0.2152, 0.7848]; val nuevas 14/44, IC [0.2000, 0.4656]; recall Sadness Female 0.300 [0.11, 0.60] y Male 1.000 [0.65, 1.00]; grupo 0 70.9 % [63.9, 77.0] |
| R5: eps del codo k-distancia; grupos, % ruido, silueta sin ruido | Cumplido con justificación | Celda 19: codo objetivo (punto más alejado de la recta entre extremos) para cada min_samples, marcado en la figura. Celda 21 lo explica. **Decisión del equipo: mantener eps=1.05 (silueta); el codo con k=30 (eps 1.324) deja 1 solo grupo y no segmenta; la alternativa ms=25/eps=1.075 da 3 grupos, 10.4 % de ruido y silueta 0.192 (celda 19)** | eps 1.05, 6 grupos, 18.72 % ruido, silueta 0.2529; codo sin cuantificar | Igual; codo k=30 en eps 1.324 (1 grupo, 5.2 % ruido) |
| R10: línea base trivial del clasificador (validador C1) | Corregido | Celda 27: `DummyClassifier(strategy='most_frequent')` ajustado solo con train y azar uniforme 1/6; el modelo final no se reajusta ni vuelve a predecir (reutiliza `y_pred_test`). Conclusión celda 28 y checklist 43 actualizados | No calculada | Mayoritaria (Happiness): F1 macro 0.0380 en test (exactitud 0.1287), 0.0444 en las 8 nuevas de test (0.1250), 0.0714 en las 44 nuevas de val (0.2727); azar 0.1667. Modelo en test nuevas: 0.4111 |

## Cifras que cambiaron respecto al notebook previo

Solo los tiempos de ejecución, que dependen de la máquina:

Valores de la última corrida (la que incluye la línea base R10):

- GridSearchCV: logística 2.99 → 2.71 s, árbol 1.13 → 1.18 s, SVM 1.22 → 1.37 s; Optuna 2.91 → 3.10 s (celdas 26 y 28).
- Fase 10 (celdas 37 y 38): pandas CSV 0.0985 → 0.0974 s, pandas Parquet 0.0812 → 0.0662 s, Polars CSV 0.0362 → 0.0448 s, Polars Parquet 0.0090 → 0.0068 s, DuckDB CSV 0.1492 → 0.1562 s, DuckDB Parquet 0.0176 → 0.0181 s. El orden entre motores no cambia; "unas 9 veces" pasa a "unas 10 veces" (Polars frente a pandas en Parquet).

Las demás cifras deterministas (limpieza, EDA, regresiones, fuga, DBSCAN, PCA, F1 de CV y de test, SHAP, sesgo) salen iguales.

## Pendiente fuera del notebook

- La guía `Ejercicio_Practico_Semana1_Guia_G9.docx` (no modificada) todavía dice que no se verificó si el test repite perfiles de train (párrafo de la Tabla 13) y no incluye intervalos de Wilson ni el codo objetivo. Si se usa en el informe, ese texto debe actualizarse con 93 de 101 y F1 0.4111 en las 8 filas nuevas.
- R5 estricto: tomar eps directamente del codo dejaría min_samples=25, eps=1.075, 3 grupos, 10.4 % de ruido y silueta 0.192 (lo que imprime la celda 19). Decisión del equipo: se mantiene ms=30, eps=1.05 (opción A).

Figura: `fases_pi/faseA_figura.png` (curva k-distancia k=30 con eps=1.05 y el codo en 1.324; salida de la celda 19).
