# Fase B: correcciones del notebook de Semana 2

Notebook: `notebooks/Ejercicio_Practico_Semana2.ipynb` (respaldo previo: `notebooks/Ejercicio_Practico_Semana2_original.ipynb`).
Ejecución: Restart & Run All con nbclient sobre una copia, kernel `.venv312` (Python 3.12.13, networkx 3.7), 14 celdas de código con execution_count de 1 a 14, sin errores. Fase 9 con Qwen/Qwen2.5-0.5B-Instruct en CPU (caché local de Hugging Face). Formato JSON conservado (indent 1, CRLF; la escritura y relectura sin cambios reproduce el archivo byte a byte). Los índices de celda empiezan en 0.

| Comentario / regla | Qué cambió | Antes | Después |
|---|---|---|---|
| Canvas S2: retirar la arista oculta antes de calcular popularidad y similitudes | **Ya cumplido, sin cambios.** Celda 29, `puntajes(R, h)`: `I2[:, h] -= 1.0` (co-ocurrencia, Jaccard, coseno) y `s[h] -= 1.0` (tamaño usado por Jaccard, coseno y la línea base de popularidad) antes de puntuar. Control con y sin fuga en las celdas 31 a 33. | Celda 32: hasta +12.9 pts de hit@5 sin el bloque Azure+microsoft | Igual: co-ocurrencia hit@5 0.713 sin fuga frente a 0.842 con fuga (+12.9) sin el bloque; con todos, hasta +3.1; popularidad +0.0 a +1.2 |
| R6: puentes = `nx.articulation_points` de la proyección | **Ya cumplido, sin cambios.** Celda 20 imprime la lista y el efecto de retirar cada punto; la celda 29 la combina con la intermediación. Se citó la lista en la conclusión (celda 21). | 8 puntos: apache, babel, facebook, google, microsoft, python, reactjs, vuejs | Igual |
| R7: leave-one-out sin fuga; hit@k del recomendador y de la línea base sobre el grafo sin la arista | **Ya cumplido, sin cambios.** Ver la primera fila. `tam[R]` no se descuenta, lo cual es correcto porque m sigue en R. | — | — |
| R8: normalizar nombres de nodo con conteo antes y después | **Corregido (v2, decisión del equipo).** Celda 8. Organizaciones: `normalizar()` completa (minúsculas, sin espacios ni puntos). Miembros: solo `str.strip()`. Se imprimen los conteos, los nombres afectados por el strip, las fusiones generadas y los 7 grupos que se fusionarían si se normalizaran los miembros (no aplicado). Celdas 14 y 29: `'Azure'` pasa a `'azure'`, porque la normalización de organizaciones lo exige. | Orgs 41 (Azure, NVIDIA, freeCodeCamp con mayúsculas); miembros 8 878; membresías 9 629 | Orgs 41 → 41, sin fusiones (azure, nvidia, freecodecamp); miembros 8 878 → 8 878 (20 nombres con espacios en los extremos, 0 fusiones); membresías 9 629 → 9 629 |
| R10: todo modelo contra la línea base de popularidad | **Ya cumplido, sin cambios.** Celda 29: co-ocurrencia, Jaccard, coseno y popularidad, hit@1/3/5, 10 semillas, con todos los miembros y sin el bloque azure+microsoft. | Sin el bloque: 0.337/0.625/0.713 frente a 0.131/0.249/0.478 | Igual |
| Leiden no reproducible con nodos `str` | **Ya tratado; se documentó mejor.** Celda 26: Leiden y LPA corren sobre `P_int` (nodos enteros en orden alfabético) con `seed` fijo y `metric="modularity"`; hay 20 semillas con ARI y NMI frente a la semilla 42. Se amplió el comentario de la celda 26 y la conclusión de la celda 27. | Leiden Q 0.2888, 0.2889 ± 0.0001, ARI 0.961 | Leiden Q 0.2890, 0.2882 ± 0.0008, ARI 0.859, NMI 0.925 (cambia porque el orden alfabético de los nodos cambió al pasar a minúsculas) |
| Conclusiones markdown | Se reescribieron las celdas 9, 12, 15, 18, 21, 24, 27, 30, 33 y 39 con las cifras de la nueva ejecución. | — | — |

## Nota sobre la decisión del equipo (R8)

Primero se aplicó la normalización completa también a los miembros (versión 1). Esa versión fusionaba 7 grupos: Ben/ben, Chris/chris, Dan/dan, Greg/greg, Lin/lin, Lei Xu/LeiXu y Sarah/"sarah ". Casi todos son nombres de pila de personas probablemente distintas, en organizaciones distintas. Se crearon 3 aristas falsas en la proyección (google–facebook y google–reactjs por Dan/dan; apache–pytorch por Lin/lin), "dan" pasó a 4 organizaciones y el tercer puente del desafío cambió de facebook a google. El equipo decidió normalizar por completo solo las organizaciones y aplicar a los miembros únicamente `strip()`. El strip no genera fusiones ("sarah " pasa a "sarah", que no existía; "Sarah" sigue siendo otro nodo), así que la estructura del grafo es idéntica a la original.

## Evidencia adicional de la auditoría (scripts fuera del notebook)

- **Variación de Leiden entre procesos.** Leiden se ejecutó con semilla 42 y `metric="modularity"` con `PYTHONHASHSEED` de 0 a 7, sobre nodos `str` y sobre nodos enteros, y dio la misma partición en todas las corridas. Con NetworkX 3.7, la partición depende del **orden de inserción de los nodos** y de la semilla, no del hash. Esto se ve en el propio notebook: al pasar Azure, NVIDIA y freeCodeCamp a minúsculas cambia el orden alfabético de P, y con él cambian las particiones de Louvain, Leiden y LPA y la curva aleatoria de robustez, aunque la red sea la misma. `P_int` se conserva como salvaguarda.

## Cifras que difieren del notebook original (backup `_original`)

La estructura (bipartito, proyección, centralidades, articulación, hit@k, control de fuga y puentes) es **idéntica**. Solo cambian los nombres (azure, nvidia, freecodecamp) y las cifras que dependen del orden de los nodos:

- Fase 5: el aleatorio cae bajo el 50 % con el 26.8 % (11 de 41; antes 31.7 %, 13 de 41); gigante tras 4 eliminados aleatorios 0.694 (antes 0.711). Dirigidos sin cambios. La figura `faseB_figura.png` es la curva nueva.
- Fase 6: orden de BFS y DFS (azure ahora se ordena tras atom); los tiempos varían por ejecución.
- Fase 7: Louvain Q 0.2890 → 0.2880 (comunidades de 16/13/4, antes 18/11/4; apache, elastic y nvidia pasan a la comunidad de google); Leiden 0.2888 → 0.2890 (18/11/4, antes 19/10/4); LPA 0.020 → 0.119 (12 comunidades); Girvan-Newman sin cambios (0.081, 19). Estabilidad en 20 semillas: Louvain 0.2886 ± 0.0005, ARI 0.807 (antes 0.2883 ± 0.0005, 0.791); Leiden 0.2882 ± 0.0008, ARI 0.859 (antes 0.2889 ± 0.0001, 0.961).
- Fase 9: tiempos (normal 0.0032 s; paralelo 0.0110 s; 5.41x y 6.16x) y texto del LLM (comunidades distintas en el prompt).
- **Organizaciones puente: microsoft, babel y facebook (igual que el original).**
