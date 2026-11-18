# Evidencia Semana 2 (notebook Ejercicio_Practico_Semana2.ipynb)

Origen: indices de celda del .ipynb (0-based). 45 celdas, 0 salidas con error. Se agrego el control con/sin fuga en las celdas 31-33; los indices posteriores a 30 se desplazaron +3 respecto a la version anterior.
Nota global: ninguna figura de S2 se guarda en `notebooks/figuras/` (esa carpeta solo tiene figuras de S3); todos los graficos son salidas inline (`plt.show()`), sin `savefig`.

## Fase 1 - Carga, limpieza y modelado (celdas 8, 9)
Pregunta: Nodos, aristas, densidad y n.o de miembros multi-organizacion. Por que un grafo bipartito?
- Nulos: member = 946, Organisation = 0. Filas duplicadas (`duplicated()`) = 942. Filas utiles tras `dropna().drop_duplicates()` = 9629 (cel. 8).
- Conclusion (cel. 9): 10 598 filas originales; 946 nulos + 23 duplicados exactos entre las validas (942 cuenta tambien nulos repetidos).
- Bipartito: 8919 nodos (41 orgs, 8878 miembros), 9629 aristas, densidad bipartita 0.02645, miembros multi-org = 724 (8.2 %).
- Figuras: ninguna.
- Negocio: orgs solo se vinculan via desarrolladores compartidos; los 724 son el unico canal de transferencia.
- Huecos: la justificacion de "por que bipartito" esta solo en el markdown (cel. 9), no hay evidencia de codigo. No se muestra tabla de nulos/duplicados del total 10 598 (el 10 598 sale solo en el texto, no en una salida).

## Fase 2 - Proyeccion y visualizacion (celda 11, 12)
Pregunta: Que pares comparten mas miembros y que muestra la visualizacion?
- Top 5 pares: Azure-microsoft 555; flutter-google 15; microsoft-nodejs 15; apache-microsoft 10; babel-nodejs 10.
- Proyeccion: 41 nodos, 84 aristas. 33 de 41 orgs con al menos un desarrollador puente; 724 puente.
- Texto (cel. 12): 545 de 724 puente pertenecen solo a Azure y microsoft.
- Figuras (inline, sin archivo): (1) proyeccion ponderada completa, 41 nodos con todas las etiquetas y todas las aristas, sin filtrar por peso; (2) subgrafo bipartito de miembros multi-org.
- Negocio: puenteo casi interno entre dos marcas del mismo grupo; puentes entre ecosistemas pocos y de peso 10-15.
- Huecos: la rubrica pide "aristas filtradas por peso, etiquetas solo en nodos relevantes"; el notebook dibuja TODAS las etiquetas y aristas (cel. 11, decision explicita). El 545 se afirma en el texto, no se imprime en codigo. La figura de proyeccion queda dominada por la arista de 555.

## Fase 3 - Centralidades (celdas 14, 15)
Pregunta: Top-3 por cada centralidad (incl. PageRank) y Spearman. Por que difieren?
- Componente gigante 33 de 41; vector propio via `eigenvector_centrality_numpy` (el iterativo fallo y se uso el numpy).
- Top-3: Grado microsoft 0.4750, nodejs 0.3500, google 0.3250. Cercania microsoft 1.2425, Azure 1.2396, nodejs 1.2199. Intermediacion microsoft 0.4011, nodejs 0.2895, babel 0.1987. Vector propio microsoft 0.7069, Azure 0.7065, nodejs 0.0232. PageRank microsoft 0.2266, Azure 0.2000, google 0.0653. Vector propio log: microsoft 0.5191, Azure 0.4289, nodejs 0.3548.
- Spearman (gigante): grado-cercania 0.808; grado-interm. 0.715; grado-VP 0.874; grado-PR 0.788; cercania-interm. 0.645; cercania-VP 0.735; cercania-PR 0.846; interm.-VP 0.637; interm.-PR 0.791; VP-PR 0.594. VP crudo vs log 0.936.
- Proporcion Azure+microsoft en VP: 0.9989 crudo, 0.4534 log.
- Figuras: ninguna.
- Negocio: version log responde mejor la pregunta; PageRank reparte influencia con amortiguacion; VP lo domina la arista de 555; intermediacion mide control de rutas.
- Huecos: el texto dice que el iterativo no converge pero la celda solo imprime "vector propio: numpy" (no muestra el error ni la distribucion de pesos). Cercania > 1 (no normalizada por la distancia 1/peso), no se explica. No hay heatmap de Spearman. PageRank vs VP: explicacion breve.

## Fase 4 - Componentes y propiedades (celdas 17, 18)
Pregunta: Componentes, aisladas, clustering, diametro y longitud media. Que dicen del ecosistema?
- 9 componentes; 8 aisladas: airbnb, bitcoin, docker, laravel, opencv, plotly, redis, udacity.
- Gigante 33/41. Ponderado (1/peso): diametro 2.8667, long. media 1.1763. En saltos: diametro 5, long. media 2.3011.
- Clustering ponderado promedio 0.0017; sin pesos 0.2809; transitividad 0.3761.
- Bipartito: miembros grado 1 = 8154 (91.8 %), grado >= 2 = 724, maximo 3. Orgs: mediana 51, maximo 3916 (microsoft).
- Figura inline: histograma de grado (miembros, log) + tamano por organizacion.
- Negocio: red pequena, cualquier org alcanza otra en pocos pasos; cohesion local en triangulos; microsoft concentra 3916 miembros.
- Huecos: ninguno relevante. El clustering ponderado 0.0017 se explica como artefacto de escala (sin evidencia numerica adicional).

## Fase 5 - Resiliencia y robustez (celdas 20, 21)
Pregunta: Con que fraccion se rompe la gigante, dirigido vs aleatorio? Puntos de articulacion?
- 8 puntos de articulacion: apache, babel, facebook, google, microsoft, python, reactjs, vuejs.
- Efecto de retirarlos (gigante original 33): babel [30,2], facebook [30,1,1], microsoft [30,1,1], apache/google/python/reactjs/vuejs [31,1].
- Gigante inicial 0.805 del total (eje sobre 41 nodos). Umbral <50 % del total: aleatorio (media 30 semillas) 31.7 % (13 de 41); dirigido grado adaptativo 14.6 % (6); dirigido intermediacion adaptativo 9.8 % (4).
- Con 4 nodos (10 %) eliminados: aleatorio 0.711, grado 0.585, intermediacion 0.488.
- Figura inline: curvas de robustez (aleatorio +- sd, grado, intermediacion).
- Negocio: fragilidad local, no colapso; red tolera mejor fallos aleatorios; intermediacion identifica los nodos criticos.
- Huecos / aviso: el umbral usado es "gigante < 50 % del total de 41" y la gigante parte de 0.805, no de 1.0; la guia pregunta "se rompe la gigante" sin definir umbral, asi que conviene aclarar la definicion al redactar. Ataque dirigido calculado sin pesos (topologia).

## Fase 6 - Caminos (celdas 23, 24)
Pregunta: Orden BFS vs DFS, camino Dijkstra y por que FW no escala.
- Desde microsoft alcanza 33 de 41; vecinos directos 19.
- BFS primeros 8: microsoft, Azure, apache, atom, babel, django, elastic, ethereum. DFS primeros 8: microsoft, Azure, apache, NVIDIA, atom, babel, elastic, nodejs.
- Dijkstra microsoft -> google: ruta directa, distancia 0.2000 (peso 5).
- FW vs 41 Dijkstra: dif. max. 4.44e-16; pares con camino 1097, sin camino 584; tiempo FW 0.0066 s vs 0.0020 s (41 Dijkstra).
- Escalabilidad: n=41 -> 68 921 ops; n=10 000 -> 1e12; n=1 000 000 -> 1e18. FW O(n^3) tiempo, O(n^2) memoria; Dijkstra O(m + n log n).
- Figuras: ninguna.
- Huecos: el Dijkstra elegido (microsoft-google) es una arista directa, ruta trivial de 1 salto; no se muestra un camino multi-salto. En esta red FW es mas lento que Dijkstra solo por poco (ms), los tiempos no ilustran el problema de escala (solo el texto).

## Fase 7 - Comunidades (celdas 26, 27)
Pregunta: Modularidad, n.o comunidades, estabilidad y tiempo por algoritmo. Cual eligen?
- Seed 42: Louvain Q 0.288987, 11 com., mayor 18, singletons 8, desconectadas 0, 0.0025 s. Leiden Q 0.288812, 11, mayor 19, 8, 0, 0.0052 s. Label Propagation Q 0.020267, 11, mayor 29, 8, 0, 0.0009 s. Girvan-Newman Q 0.081218, 19 com., mayor 11, 11 singletons, 0, 0.1465 s.
- 20 semillas: Louvain Q 0.2883 +- 0.0005, ARI 0.791, NMI 0.895; Leiden 0.2889 +- 0.0001, ARI 0.961, NMI 0.976; LPA 0.1218 +- 0.0421 (12.0 +- 0.3 com.), ARI 0.716, NMI 0.889; GN determinista.
- Comunidades Louvain: C0 (18) NVIDIA, apache, atom, aws, babel, elastic, facebook, graphql, huggingface, jquery, nodejs, nuxt, pytorch, rails, reactjs, reduxjs, serverless, vuejs; C1 (11) ansible, deepmind, django, firebase, flutter, freeCodeCamp, golang, google, grafana, prometheus, python; C2 (4) Azure, ethereum, microsoft, mongodb; 8 singletons (aislados). Leiden: C0 (19, suma freeCodeCamp), C1 (10), C2 (4).
- Decision: Leiden (igual Q, mas estable, comunidades conectadas); Louvain alternativa.
- Figuras: ninguna.
- Huecos / errores a revisar: (1) Q de Louvain esta calculada sobre toda P con 8 singletons: la modularidad 0.289 es de red con 3 comunidades reales; la guia no pide separar. (2) Texto afirma Leiden "garantiza comunidades conectadas": el notebook solo verifica que 0 estan desconectadas en esta corrida. (3) No hay visualizacion de comunidades. (4) Louvain y Leiden usan "peso" como afinidad y Girvan-Newman usa 1/peso: criterios distintos, no se discute. (5) El enunciado en la conclusion "Louvain 0.2890" vs tabla 0.288987 coincide.

## Fase 8 - Recomendador y desafio (celdas 29, 30)
Pregunta: hit@k vs popularidad y las 3 organizaciones puente.
- 724 miembros evaluables, 10 semillas, sin fuga (se retira la arista antes de puntuar). hit@1/3/5 todos: Co-ocurrencia 0.837/0.908/0.929; Jaccard 0.818/0.897/0.930; Coseno 0.822/0.893/0.928; Popularidad 0.786/0.815/0.871.
- Sin los 545 miembros solo en {Azure, microsoft} (n=179): Co-ocurrencia 0.337/0.625/0.713; Jaccard 0.264/0.582/0.709; Coseno 0.277/0.564/0.703; Popularidad 0.131/0.249/0.478.
- Desafio 1 (rango = promedio de rangos intermediacion y fragmentacion): microsoft (interm. 0.4011, gigante tras retirar 30, 11 componentes, 2 desconectadas, rango 1.5); babel (0.1987, 30, 10, 2, 2.5); facebook (0.1141, 30, 11, 2, 3.5); google 0.1513 (31, 10, 1, rango 5.0); python 0.0782; apache 0.0667; reactjs/vuejs 0.0397.
- Top 3 puente: microsoft, babel, facebook. Desarrolladores con mas membresias: 5 empatados en 3 (Nicolas Garnier, Ryan Wilson, Matthew Dempsky, Daniel Lo Nigro, dan).
- Figuras: ninguna.
- Negocio: la ventaja global esta inflada por el bloque Azure/microsoft; sin el, el recomendador supera claramente a popularidad.
- Huecos: el desafio 1 pide "3 puente con evidencia de centralidad y robustez": cubierto, pero el criterio de rango es propio (el top-3 depende de el: nodejs es 2.o en intermediacion y queda fuera). Desafio 3 (comparar comunidades) en fase 7. Desafio 4 (etica) en fase final. No hay grafico del recomendador. El recomendador puntua con la similitud de orgs, no hay ejemplo concreto de recomendacion para un desarrollador.

## Fase 9 - Backends y mini GraphRAG (celdas 36, 38, 39)
Pregunta: tiempo normal vs parallel y respuesta GraphRAG; alucino el LLM?
- Backends: ['parallel'], 16 nucleos. P real (41 nodos): normal 0.0027 s; paralelo 1.3330 s primera llamada, 0.0110 s mejor de 5; dif. max. 5.55e-17; aceleracion 0.24x. gnp n=500 (~2437 aristas): normal 0.3612 s, paralelo 0.0742 s, 4.86x, dif. 1.04e-17. gnp n=1500 (~7489): normal 4.0057 s, paralelo 0.6296 s, 6.36x, dif. 9.97e-18.
- LLM: Hugging Face Qwen/Qwen2.5-0.5B-Instruct (sin Ollama). Respuestas truncadas (max_tokens 100/250). Prompts en espanol; el modelo respondio de forma generica a las comunidades de 1 org.
- Fidelidad: 10 orgs del dataset mencionadas (Azure, airbnb, aws, docker, ethereum, flutter, google, microsoft, nodejs, python). 21 entidades fuera del dataset: Algunos, Amazon, Blockchain, Control, Dart, DevOps, Git, Java, JavaScript, Jupyter, Lambda, Machine, Notebook, PyCharm, React, Ruby, Systems, TI, VM, Version, Virtual. Veredicto automatico: posible alucinacion.
- Negocio: backend paralelo conviene desde cientos de nodos; en la red real no compensa. El LLM pequeno no es fiel (describe Node.js como lenguaje, atribuye Docker a la comunidad de bitcoin, trunca).
- Citas: Edge et al. (2024) y "Toda base de datos es un grafo" citados en la conclusion.
- Figuras: ninguna.
- Huecos / errores: (1) Los resumenes usan las 5 mayores comunidades de Louvain, 2 de ellas singletons (airbnb, bitcoin): resumen generico sin valor. (2) El detector de fidelidad es ruidoso: marca palabras comunes ("Algunos", "Control", "Machine", "Version", "Systems", "TI", "Virtual") y tecnologias que no son organizaciones; "Amazon" ~ aws. El conteo de 21 no son 21 alucinaciones de organizaciones. Por la propia guia ("organizaciones que no estan en los datos") la respuesta correcta es: si menciona entidades ajenas (Amazon/AWS Lambda, Jupyter, PyCharm, Dart, React) pero no organizaciones de GitHub inventadas de forma clara; requiere revision manual. (3) La respuesta global esta cortada en "8. **Airbnb". (4) La sintesis de tiempos de la guia ("desde que tamano conviene") solo tiene 2 puntos sinteticos (500, 1500); no hay punto de cruce entre 41 y 500. (5) Cita bibliografica sin referencia completa (solo "Edge et al., 2024").

## Fase final - Aplicacion profesional (celdas 41-43)
- Rodrigo Soria (Bomberos GADM Riobamba): bipartito sectores-riesgos; Louvain + grado; etica: sesgo de asignacion / equidad. 239 palabras (cuerpo).
- Kevin Ambuludi (credito/cobranza): bipartito clientes-comportamientos; Louvain; etica: discriminacion por asociacion y datos personales. 323 palabras.
- Christian Ruiz (obligaciones patronales): bipartito empleadores-indicadores; Louvain; etica: sesgo del historial de inspecciones, revision humana, inspecciones aleatorias. 290 palabras.
- Huecos: guia pide 150-250 palabras; Kevin (323) y Christian (290) exceden; Rodrigo (239) esta dentro. Los tres usan casi el mismo diseno (bipartito + Louvain), sin centralidad/robustez, y ninguno justifica la eleccion frente a Leiden (elegido en fase 7). Roles en la tabla de integrantes (cel. 0) vacios; "Grupo N.o: ___" sin completar (la rubrica pide roles declarados).

## Resumen de huecos y errores (para la guia)
1. Ninguna figura S2 guardada en `notebooks/figuras/` (todas inline): si la guia necesita imagenes, hay que exportarlas.
2. Fase 1: justificacion de bipartito solo en texto.
3. Fase 2: el grafico dibuja todas las etiquetas/aristas (rubrica pide filtrar).
4. Fase 3: no se muestra el fallo de convergencia del vector propio iterativo; cercania > 1 sin explicar.
5. Fase 5: umbral de "ruptura" definido como gigante < 50 % del total (parte de 0.805).
6. Fase 6: Dijkstra elegido es arista directa (trivial); tiempos no ilustran escala.
7. Fase 7: Q global incluye 8 singletons; "garantiza conectadas" no verificado mas alla de esta corrida.
8. Fase 8: top-3 puente depende de un rango propio (nodejs queda fuera pese a 2.a intermediacion); la ventaja del recomendador global esta inflada (0.908 vs 0.625 sin Azure/microsoft).
9. Fase 9: detector de alucinacion ruidoso, 2 de 5 comunidades son singletons, respuestas truncadas.
10. Fase final: Kevin (323) y Christian (290) exceden 250 palabras; roles y numero de grupo vacios.
