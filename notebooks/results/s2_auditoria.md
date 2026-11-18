# Auditoria S2 - Modo A (notebook vs guia)

Alcance: `notebooks/Ejercicio_Practico_Semana2.ipynb` (45 celdas; 14 de codigo, todas ejecutadas con execution_count 1-14 en orden) contra `guia_s2.txt`. Verificado tambien contra `organization.csv` en cache de kagglehub.

## Resultado general
Las 9 fases cumplen lo pedido por la guia. No hay celdas sin ejecutar, no hay salidas con `error`, y las cifras de las conclusiones coinciden con las salidas. Los unicos stderr son un TqdmWarning (celda 8) y las barras "Loading weights" del modelo HF (celda 36), ambos inocuos.

## CRITICO
1. **Desafio punto 4 (implicacion etica de analizar redes de personas reales) no esta respondido sobre el caso GitHub.** Ninguna conclusion de Fases 1-9 lo declara. La unica etica del notebook esta en la Fase final, y habla de bomberos, cobranza e inspecciones laborales, no de desarrolladores de GitHub. Ademas la celda 29 imprime nombres reales de desarrolladores ('Nicolas Garnier', 'Ryan Wilson', ...) sin comentario de privacidad. Falta un parrafo en la conclusion de la Fase 8 (o 9). Esto afecta a la rubrica "Desafio resuelto" (6 pts).

## Contradicciones cifras vs conclusiones (revisadas, sin hallazgo CRITICO)
- **942 vs 23 duplicados: coherente.** El CSV tiene 10 598 filas y 946 nulos. `df.duplicated()` da 942, pero `dropna().duplicated()` da 23. 10 598 - 946 - 23 = 9 629, igual al "Filas utiles" de la celda 8. La conclusion de la celda 9 lo explica correctamente.
- Fase 2: 545 de 724 puente solo en Azure+microsoft. La Fase 8 da 545 de 724, y la arista de 555 incluye a 10 miembros con otras organizaciones. Coherente.
- Fase 3: todas las cifras de top-3 y Spearman (0.594 a 0.874) coinciden con la salida.
- Fase 5: 31.7 %, 14.6 % y 9.8 %; 0.711, 0.585 y 0.488. Coinciden.
- Fase 7: Q 0.2890 (Louvain) y 0.2888 (Leiden), ARI/NMI y Q de LPA coinciden con la tabla.
- Fase 8: hit@k con y sin Azure+microsoft y top-3 puente (microsoft, babel, facebook) coinciden con las salidas.
- Fase 9: aceleraciones 0.24x, 4.86x y 6.36x coinciden con la conclusion.

## ADVERTENCIA
1. **Roles y grupo sin declarar** (celda 0): "Grupo N.º: ___" y la columna "Rol en el equipo" esta vacia para los 3 integrantes. La rubrica pide roles declarados (3 pts). La guia lista 4 integrantes y el notebook tiene 3; confirmar que es correcto.
2. **Fase 9b, "5 comunidades mas grandes".** Las comunidades 4 y 5 son airbnb y bitcoin, singletons aislados, por lo que sus resumenes son genericos. La conclusion lo reconoce (celda 39), pero para mejorar el GraphRAG se podrian tomar las 3 comunidades reales mas la de Leiden, o justificar el criterio mas explicitamente.
3. **Fidelidad sobrestimada (celda 34, 35).** El detector marca como "no pertenecen al dataset" a React, Git, Java, Ruby, Dart y Jupyter. Son tecnologias reales, no organizaciones inventadas. La conclusion los lista como evidencia de alucinacion. Los errores de contenido reales son otros: Node.js descrito como "lenguaje", Docker mencionado en el resumen de bitcoin y "Amazon" como organizacion ausente (el dataset tiene `aws`). Conviene separar "alucinacion de organizacion" de "tecnologia mencionada".
4. **Salidas del LLM truncadas** por `max_tokens=100` y `250` (cortan en "Airbnb" y en "Ethereum: Un blockchain"). Con Qwen 0.5B (sin Ollama) la calidad es muy baja; esto es declarado en la conclusion.
5. **Legibilidad de visualizaciones (rubrica de 6 pts).** La celda 11 dibuja las 84 aristas y las 41 etiquetas sin filtrar; la propia celda lo declara ("sin filtrar"). La rubrica pide "aristas filtradas por peso, etiquetas solo en nodos relevantes".
6. **Fase 7: "Leiden y Louvain coinciden"** (celda 27) es cierto en Q y en n.º de comunidades (11), pero no en composicion: Leiden da una comunidad mayor de 19 (suma freeCodeCamp) y otra de 10, frente a 18 y 11 de Louvain.
7. **Versiones registradas.** Solo se imprime networkx y pandas (celda 8); la rubrica pide "versiones registradas". Faltan scipy, sklearn, nx-parallel y la version del modelo.

## SUGERENCIA
1. Fase 6: Dijkstra microsoft->google es una arista directa (distancia 0.2). No demuestra un camino de varios saltos; conviene elegir un par como `google`->`vuejs`.
2. Fase 5: los ataques usan intermediacion y grado sin pesos, mientras que la Fase 3 usa intermediacion ponderada por 1/peso. Esta decision esta declarada en el codigo; anadirla a la conclusion.
3. Fase 9a: el umbral "algunos cientos de nodos" se apoya en solo n=41, 500 y 1 500. Una prueba en n=100 y 250 daria el punto de cruce.
4. El nombre de archivo de entrega debe ser `grupoN_Semana2_Ejercicio.ipynb`; el actual es `Ejercicio_Practico_Semana2.ipynb`.

## Cumplimiento por fase
| Fase | Estado | Nota |
|---|---|---|
| 1 | OK | Nulos, duplicados, bipartito, densidad, 724 puente, justificacion del bipartito. |
| 2 | OK (visual sin filtrar) | Proyeccion ponderada, top-5 pares, subgrafo bipartito. |
| 3 | OK | 5 centralidades, 1/peso en cercania e intermediacion, Spearman, vector propio en la gigante con respaldo numpy y variante log, PageRank discutido. |
| 4 | OK | Componentes, aisladas, diametro, longitud media, clustering, distribucion de grado. |
| 5 | OK | Aleatorio (30 semillas) vs dirigido por grado e intermediacion, puntos de articulacion con su efecto. |
| 6 | OK | BFS, DFS, Dijkstra, Floyd-Warshall verificado contra Dijkstra, O(n^3). |
| 7 | OK | Girvan-Newman, Louvain, Leiden con `metric="modularity"`, LPA; conectividad interna, estabilidad (20 semillas, ARI/NMI), tiempo. |
| 8 | OK (falta etica, ver CRITICO) | hit@1/3/5 vs popularidad, sin fuga, con y sin Azure+microsoft, top-3 puente. |
| 9 | OK | nx-parallel con verificacion numerica, mini GraphRAG con chequeo de fidelidad, cita de Edge et al. (2024) y de la lectura. |
| Final | OK | 3 respuestas individuales; la guia pide 4 (ver ADVERTENCIA 1). |

Checklist de cierre del notebook (8 items, todos marcados): respaldado por la evidencia, excepto que no hay un item de etica del desafio 4.
