# Respuestas escritas - Semana 2 (Fases 1 a 9)

Fuente de cifras: notebook Ejercicio_Practico_Semana2.ipynb (salidas ejecutadas). La Fase final es individual y no se redacta aquí.

## Fase 1

Cargamos organization.csv, que tiene 10 598 filas. La columna member tiene 946 nulos y Organisation tiene 0. El método duplicated() reporta 942 filas duplicadas, pero esa cifra confunde: también cuenta como duplicadas las filas con nulos repetidos. Si primero quitamos los nulos, los duplicados exactos reales son 23. La cuenta cuadra: 10 598 - 946 - 23 = 9 629 filas útiles, que pasan a ser las aristas. El grafo bipartito tiene 8919 nodos (41 organizaciones y 8878 miembros), 9629 aristas y una densidad bipartita de 0.02645. De los miembros, 724 (8.2 %) pertenecen a más de una organización.

Usamos un grafo bipartito porque hay dos tipos de entidad (organización y persona) y los vínculos solo existen entre tipos distintos. Una organización nunca se conecta directo con otra: lo hace a través de desarrolladores compartidos. Un grafo simple mezclaría ambos tipos, y el bipartito permite proyectar después sobre organizaciones sin perder quién las une. Esos 724 miembros son el único canal de transferencia entre organizaciones.

## Fase 2

Los pares que más miembros comparten son Azure-microsoft (555), flutter-google (15), microsoft-nodejs (15), apache-microsoft (10) y babel-nodejs (10). La proyección ponderada sobre organizaciones tiene 41 nodos y 84 aristas, y 33 de las 41 organizaciones tienen al menos un desarrollador puente. Un detalle clave: 545 de los 724 miembros puente pertenecen solo a Azure y microsoft.

La visualización muestra una red con un par dominante. La arista de 555 aplasta visualmente al resto, y los demás vínculos entre ecosistemas son pocos y de peso 10 a 15. Es decir, el puenteo más fuerte es casi interno, entre dos marcas del mismo grupo, y no una transferencia entre ecosistemas distintos.

Una limitación honesta: decidimos dibujar los 41 nodos con todas las etiquetas y las 84 aristas, sin filtrar por peso. Eso garantiza ver todo, pero la rúbrica pide aristas filtradas y etiquetas solo en nodos relevantes, así que el gráfico es menos legible de lo ideal. También graficamos el subgrafo bipartito de los miembros multi-organización.

## Fase 3

Trabajamos sobre la proyección, usando 1/peso como distancia en cercanía e intermediación, como indica la documentación de NetworkX (el peso es una distancia en esas medidas). Top-3 por medida:

- Grado: microsoft 0.4750, nodejs 0.3500, google 0.3250.
- Cercanía: microsoft 1.2425, Azure 1.2396, nodejs 1.2199.
- Intermediación: microsoft 0.4011, nodejs 0.2895, babel 0.1987.
- Vector propio: microsoft 0.7069, Azure 0.7065, nodejs 0.0232.
- PageRank: microsoft 0.2266, Azure 0.2000, google 0.0653.

Spearman en la componente gigante va de 0.594 (vector propio-PageRank) a 0.874 (grado-vector propio). Los rankings difieren porque cada medida mira algo distinto: grado cuenta vecinos, intermediación mide control de rutas y vector propio premia estar junto a nodos importantes. El vector propio lo domina la arista de 555: Azure y microsoft concentran 0.9989 del total. Con pesos logarítmicos baja a 0.4534 y la lista queda más útil (microsoft 0.5191, Azure 0.4289, nodejs 0.3548). PageRank reparte la influencia con amortiguación, así que no se hunde en un solo par.

## Fase 4

La proyección tiene 9 componentes: la gigante con 33 de 41 organizaciones y 8 aisladas (airbnb, bitcoin, docker, laravel, opencv, plotly, redis y udacity). En la gigante, con distancia 1/peso, el diámetro es 2.8667 y la longitud media de camino 1.1763. Contando saltos, el diámetro es 5 y la longitud media 2.3011. El clustering ponderado promedio es 0.0017, pero sin pesos sube a 0.2809, y la transitividad es 0.3761. El valor ponderado tan bajo es un efecto de escala de los pesos, no ausencia de triángulos.

En el bipartito, 8154 miembros (91.8 %) están en una sola organización y 724 en dos o más, con un máximo de 3. Las organizaciones tienen mediana de 51 miembros y microsoft llega a 3916.

Conclusión: el ecosistema es pequeño, porque cualquier organización de la gigante alcanza a otra en pocos pasos. Hay cohesión local en triángulos, hay un grupo de organizaciones totalmente desconectadas, y microsoft concentra el tamaño.

## Fase 5

Definimos la ruptura de la gigante como el momento en que cae por debajo del 50 % de los 41 nodos. Ojo: la gigante ya parte de 0.805 (33 de 41), no de 1.0. Con ese umbral, el borrado aleatorio (media de 30 semillas) rompe la red al eliminar 31.7 % (13 de 41). El ataque dirigido por grado adaptativo la rompe con 14.6 % (6 nodos) y por intermediación adaptativa con 9.8 % (4 nodos). Con 4 nodos eliminados (10 %), la gigante queda en 0.711 si es aleatorio, 0.585 por grado y 0.488 por intermediación. Los ataques dirigidos se calcularon sin pesos (solo topología).

Hay 8 puntos de articulación: apache, babel, facebook, google, microsoft, python, reactjs y vuejs. Al retirar babel la gigante queda [30, 2]; facebook y microsoft dejan [30, 1, 1]; los otros cinco dejan [31, 1].

Conclusión: la red tiene fragilidad local, no colapso total. Tolera mejor fallos aleatorios, y la intermediación identifica mejor los nodos críticos.

## Fase 6

Desde microsoft se alcanzan 33 de 41 organizaciones, con 19 vecinos directos. El BFS visita por capas: microsoft, Azure, apache, atom, babel, django, elastic, ethereum. El DFS se va en profundidad por una rama antes de volver: microsoft, Azure, apache, NVIDIA, atom, babel, elastic, nodejs.

Dijkstra de microsoft a google da ruta directa de distancia 0.2000 (peso 5). Es una arista de un salto, así que es un caso trivial, no un camino de varios saltos. Floyd-Warshall coincide con 41 corridas de Dijkstra (diferencia máxima 4.44e-16): hay 1097 pares con camino y 584 sin camino. En tiempo, FW tardó 0.0066 s y Dijkstra 0.0020 s.

Floyd-Warshall es O(n^3) en tiempo y O(n^2) en memoria. Con n = 41 son 68 921 operaciones, con n = 10 000 serían 1e12 y con n = 1 000 000, 1e18. Por eso no escala al grafo completo. Dijkstra cuesta O(m + n log n) y conviene para grafos grandes o cuando solo se necesitan caminos desde algunos orígenes (NetworkX, documentación de caminos mínimos). Con 41 nodos los tiempos no muestran el problema de escala: eso sale solo del conteo teórico.

## Fase 7

Con semilla 42, la modularidad (Q) es: Louvain 0.288987 con 11 comunidades (0.0025 s), Leiden 0.288812 con 11 (0.0052 s), propagación de etiquetas 0.020267 con 11 (0.0009 s) y Girvan-Newman 0.081218 con 19 (0.1465 s). Ninguna comunidad salió internamente desconectada. En 20 semillas: Louvain Q 0.2883 +- 0.0005 (ARI 0.791, NMI 0.895), Leiden 0.2889 +- 0.0001 (ARI 0.961, NMI 0.976) y propagación de etiquetas 0.1218 +- 0.0421 (ARI 0.716, NMI 0.889). Girvan-Newman es determinista.

Elegimos Leiden: misma modularidad que Louvain, pero más estable entre semillas y, según Traag et al. (2019), con comunidades garantizadas conectadas (en nuestra corrida lo comprobamos: 0 desconectadas). Louvain queda como alternativa. Aclaración: Louvain y Leiden coinciden en Q y en 11 comunidades, pero no en composición (Leiden da una de 19 que suma freeCodeCamp y Louvain una de 18).

Las 3 comunidades reales de Louvain tienen 18, 11 y 4 organizaciones y hay 8 aisladas. Por eso la Q de 0.289 viene de una red con solo 3 comunidades reales. Además, Louvain y Leiden usan el peso como afinidad y Girvan-Newman usa 1/peso, así que no son del todo comparables.

## Fase 8

Evaluamos 724 miembros multi-organización, ocultando una membresía y puntuando sin fuga (se retira la arista antes de puntuar), con 10 semillas. Resultados hit@1 / hit@3 / hit@5: co-ocurrencia 0.837 / 0.908 / 0.929; Jaccard 0.818 / 0.897 / 0.930; coseno 0.822 / 0.893 / 0.928; popularidad 0.786 / 0.815 / 0.871.

Esta ventaja está inflada por el bloque Azure/microsoft. Al quitar los 545 miembros que están solo en esas dos, quedan 179 evaluables y el hit@3 de co-ocurrencia baja de 0.908 a 0.625 (hit@1 0.337, hit@5 0.713). Popularidad baja a 0.131 / 0.249 / 0.478. Sin ese bloque el recomendador sigue superando claramente a popularidad, pero con un margen más realista.

Las 3 organizaciones puente son microsoft, babel y facebook. microsoft tiene intermediación 0.4011 y, al retirarla, la gigante queda en 30 con 11 componentes. babel tiene 0.1987 y deja 30 nodos con 10 componentes. facebook tiene 0.1141 y deja 30 con 11 componentes. El criterio de rango (promedio de intermediación y fragmentación) es propio: nodejs es segundo en intermediación y queda fuera.

**Ética (desafío 4).** Estos datos son membresías de personas reales, no objetos abstractos. El notebook imprime nombres de desarrolladores (por ejemplo Nicolas Garnier y Ryan Wilson) sin ningún aviso de privacidad. El riesgo es la reidentificación: cruzar membresías con otras fuentes públicas permite perfilar a una persona. El recomendador también tiene un sesgo, porque refuerza organizaciones populares y deja a las pequeñas sin visibilidad. Mitigaciones: anonimizar con identificadores en lugar de nombres, minimizar los datos usados, reportar resultados agregados y evaluar el sesgo por popularidad antes de usar el recomendador.

## Fase 9

**Backends.** Con backend parallel (16 núcleos) en la red real de 41 nodos, normal tardó 0.0027 s y paralelo 1.3330 s la primera llamada (0.0110 s la mejor de 5): aceleración 0.24x, no compensa. Con un grafo sintético gnp de n = 500 (cerca de 2437 aristas): 0.3612 s contra 0.0742 s, 4.86x. Con n = 1500 (cerca de 7489 aristas): 4.0057 s contra 0.6296 s, 6.36x. Las diferencias máximas fueron del orden de 1e-17, así que el resultado es el mismo. Conviene desde cientos de nodos, aunque solo probamos 41, 500 y 1500, y no tenemos el punto de cruce exacto.

**GraphRAG.** Siguiendo Edge et al. (2024), resumimos comunidades con un LLM pequeño (Qwen2.5-0.5B-Instruct, de Hugging Face) y respondimos una pregunta global. Dos de las 5 comunidades más grandes de Louvain son singletons (airbnb y bitcoin), así que sus resúmenes son genéricos. Las respuestas salieron truncadas (se cortan en "8. **Airbnb").

**¿Hubo alucinación?** El detector marcó 21 entidades fuera del dataset, pero es ruidoso: cuenta React, Git y Java como ausentes, y esos son tecnologías, no organizaciones. También marca palabras comunes ("Algunos", "Control", "Version"). La evidencia sí muestra errores de contenido: describe Node.js como lenguaje y atribuye Docker a la comunidad de bitcoin. No tenemos evidencia de organizaciones de GitHub inventadas de forma clara, así que no lo llamamos alucinación de organizaciones. La lectura de tendencia ("Toda base de datos es un grafo") respalda el uso de grafos para este tipo de consulta.
