# Fase C: autoevaluación y correcciones de la Semana 3

**Notebook entregado:** `notebooks/Ejercicio_Practico_Semana3.ipynb`. Se reconstruyó a partir de `notebooks/anteriores/Ejercicio_Practico_Semana3_G9.ipynb` y después se le aplicaron las correcciones de esta fase.
**Respaldo:** `notebooks/Ejercicio_Practico_Semana3_original.ipynb` contiene lo que había antes en esa ruta, la **plantilla vacía** del 4 de noviembre, sin ejecutar y con celdas `# Completar`.
**Ejecución:** se hizo Restart & Run All completo en una copia, con el kernel `.venv312` (Python 3.12.13), el mismo con el que se ejecutó el notebook original. El último run tardó 1 561 s, terminó sin errores y dejó `execution_count` de 1 a 75 (secuencial, sin repetidos). Hubo dos corridas completas y las métricas coincidieron en las dos (con las semillas fijas son deterministas). Solo cambiaron los tiempos.
**Formato:** JSON con indent 1 y CRLF, igual que el archivo de destino. Se verificó que leer y reescribir el archivo sin cambios lo reproduce byte a byte.
**Datos:** `tweet_emotions.csv` se carga con kagglehub, desde la caché local ya presente (`~/.cache/kagglehub/datasets/pashupatigupta`), y como alternativa desde `data/tweet_emotions.csv`. No se descargaron datos nuevos. BERTopic, MiniLM y Qwen2.5-0.5B se cargaron desde la caché local de Hugging Face.

> Advertencia de entorno: `.venv` usa Python 3.14 y le faltan spacy, pyspark, tensorflow, bertopic, sentence-transformers y umap, así que con ese entorno el notebook no corre. El notebook declara `.venv312` como kernel y con él corre completo. Spark 4.3.0 usa Java 21 (Temurin) en esta máquina.

## Estado por regla

| Regla | Estado | Evidencia (celda, índice base 0) |
|---|---|---|
| R1: toda cifra sale de una salida ejecutada | **Corregida** | Las conclusiones citaban tiempos de una corrida anterior que no coincidían con las salidas guardadas. Ejemplos: RNN «57 s» frente a 45,9 s impresos; Spark «15,7 s, 43 veces» frente a 19,1 s y 19 veces; RDD «0,6 s» frente a 27,4 s; LLM «0,030, 0,33 s/tweet, 87 %» frente a 0,023, 8,68 s y 88 %; BERTopic «47,2 %, pureza 0,296» frente a 46,9 % y 0,300; conteos de temas 173/162/225 frente a 177/173/226. Se corrigieron en las celdas 67, 80, 95, 123, 129, 145, 151, 155 y 156 con las salidas del run final. La tabla de alternativas (celda 128) ya no lleva cifras escritas a mano: los tiempos se calculan con variables y no cita cifras de la Fase 9, que se ejecuta después. Un barrido automático confirmó que cada decimal del markdown aparece en las salidas o en `results/*.json`, salvo redondeos derivados (42,7 %; 12,81; 49,3; 27,8). |
| R9: mejoras de tiempo solo si se midieron | **Corregida** | pandas/sklearn frente a Spark ya estaba medido (celda 114). CSV frente a Parquet solo comparaba tamaños, así que se añadió la medición de **tiempos de lectura** (mejor de 3) con pandas y con Spark (celda 152), que se guarda en `results/fase9.json` y se cita en la celda 156. La interpretación del RDD frente al DataFrame se invirtió para que coincida con la medición: el RDD resulta más lento. |
| R10: modelos frente a línea base; la prueba se usa una sola vez | **Corregida** | La línea base (azar 1/13 = 0,077 y clase mayoritaria con F1 de 0,027) está en la celda 56, y la de azar 1/k en las celdas 85 y 87. **Fallo encontrado:** el mejor modelo se elegía por F1 macro **de prueba** (celda 60). Ahora se elige por F1 de validación (NB 0,1428, SVM 0,1873) y el resultado no cambia (SVM). La prueba se evalúa una sola vez por modelo. En la Fase 6 se usa una vez por combinación (k, semilla) y así se declara (celda 82). |
| W8 (validador): recomendación de granularidad | **Corregida** | Antes «5 a 7 categorías» → después «7 clases», porque solo k = 7 cumple el criterio de la celda 93 (k = 5: ganancia 1,96 < 2, `cumple = False`). Solo cambia el texto markdown de las celdas 95 y 129, sin re-ejecutar; también se actualizó `decisiones.granularidad` en `faseC_cifras.json`. |
| Limpieza (`empty`, duplicados) | Cumplida | Celdas 10, 16, 19 y 21: 173 duplicados de texto, 62 textos con etiquetas contradictorias (207 filas eliminadas), 28 copias y 77 vacíos. Quedan 40 000 → 39 688 tweets. `empty` (827 tweets, ninguno con texto vacío) se conserva como 13.ª clase ruidosa. |
| F1 frente al número de clases | Cumplida | Celdas 83, 85, 87 y 93: 3 semillas, banda ±1 desviación y línea de azar 1/k. Criterio explícito (F1 ≥ 0,30 y al menos 2 veces el azar) → k = 7. Figura en `fases_pi/faseC_figura.png`. |
| LDA (y BERTopic) | Cumplida | LDA en las celdas 44, 46, 47 y 49: K = 5 por perplejidad de validación, confirmado por NPMI. BERTopic en las celdas 136 y 138: 25 temas, 46,9 % de tweets sin tema y pureza de 0,300 frente a 0,229 de LDA. |
| Réplica Spark (MLlib, SQL, ventana, MapReduce) | Cumplida | Celdas 100–112: MLlib con F1 macro de 0,142 y la misma partición; `explain()`; Spark SQL; `row_number()` y conteo con RDD. Tiempos medidos en las celdas 114 y 152. |

## Cambios de código (3 celdas)
1. **Celda 60:** `mejor_nombre` se elige por F1 de validación (`f1_val_modelos`) y no por F1 de prueba.
2. **Celda 152 (y 154):** se mide el tiempo de lectura de CSV y Parquet (pandas `read_csv`/`read_parquet`; Spark leer y contar por emoción), se imprime en `tabla_lectura` y se guarda en `res9['parquet']['tiempos_lectura_s']`.
3. **Celda 128:** la tabla de alternativas calcula los tiempos de NB y la razón RNN/SVM con variables, y quita las cifras de la Fase 9.

## Cifras clave (run final; detalle y celda de origen en `faseC_cifras.json`)
- **13 clases, prueba:** SVM con F1 macro de 0,198 (accuracy 0,270), NB con 0,144 (0,299), RNN con 0,149 (0,176). Las líneas base son azar 0,077 y clase mayoritaria 0,027. Tiempos de entrenamiento en esta corrida: NB 1,07 s, SVM 1,84 s y RNN 57,8 s.
- **F1 macro del SVM según k** (media de 3 semillas): 0,543 / 0,393 / 0,316 / 0,258 / 0,229 para k = 3/5/7/9/11; el azar es 0,333 / 0,200 / 0,143 / 0,111 / 0,091. k recomendado: 7 (único k que cumple; k = 5 queda en 1,96 veces el azar).
- **Spark:** MLlib NB con F1 macro de 0,142. Flujo completo de 28,1 s frente a 0,75 s de sklearn NB (37 veces más). El RDD tardó 27,5 s y el DataFrame 5,5 s. Lectura con Spark: CSV 0,21 s y Parquet 0,16 s; con pandas, 0,009 s y 0,007 s. Tamaño: CSV de 3,43 MB y Parquet de 2,25 MB.
- **Fase 9:** embeddings con F1 macro de 0,159 (97 s de codificación + 22 s de entrenamiento). LLM de 0,5B con F1 de 0,023, 4,28 s por tweet y 88 % de respuestas fuera de la lista. PSI de 0,35 (cambio fuerte).

## Decisiones (granularidad corregida por W8; el resto sin cambios)
- **Granularidad:** 7 clases, no 13 (corrección W8).
- **Modelo:** SVM lineal con TF-IDF.
- **Spark:** no se justifica con este volumen (2,6 MB de texto); sí a partir de unos 1 millón de tweets por día con un año de historia (estimación con supuestos declarados en la celda 116).

## Pendientes y avisos
- `notebooks/results/*.json` y `notebooks/figuras/*.png` siguen siendo los de la corrida del 17-nov. No se sobrescribieron porque solo se pidió escribir en `fases_pi/` y en el notebook. Las métricas son idénticas, pero los tiempos difieren. Los JSON de esta corrida están en el scratchpad (`s3c/run2/results`).
- Las fases 1 y 3 escriben `results/fase*.json` sin `encoding='utf-8'` (en Windows quedan en cp1252). No se corrigió porque es menor y no afecta las cifras.
- El nombre del notebook en git cambió: `notebooks/Ejercicio_Practico_Semana3_G9.ipynb` figura como borrado y su contenido vive ahora en `notebooks/Ejercicio_Practico_Semana3.ipynb`. La guía DOCX no se tocó.
- No se pudo ejecutar el validador de paleta de la figura (el script no produce salida con Node en este entorno). Para que la figura no dependa solo del color, cada serie se distingue también por estilo de línea, marcador y etiqueta directa.
