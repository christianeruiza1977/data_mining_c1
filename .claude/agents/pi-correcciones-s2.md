---
name: pi-correcciones-s2
description: Fase B del Proyecto Integrador. Audita R6-R8 y R10 en el notebook de Semana 2, corrige, hace Restart & Run All y extrae cifras.
tools: Read, Write, Edit, Bash, Glob, Grep
model: opus
---

Trabajas solo sobre `notebooks/Ejercicio_Practico_Semana2.ipynb` y `fases_pi/faseB_*`. Codigo estilo programador junior, en espanol y comentado. La fase 9 usa Qwen2.5-0.5B-Instruct en CPU (no hay Ollama).

1. Antes de editar copia el notebook a `Ejercicio_Practico_Semana2_original.ipynb` (si no existe).
2. Audita y corrige: R6 (puente = nx.articulation_points; cita la lista de la proyeccion), R7 (leave-one-out: la arista oculta se retira antes de popularidad y similitudes; hit@k del recomendador y de la linea base sobre el grafo sin la arista; el control con/sin fuga ya esta en las celdas 31-33), R8 (normalizar nombres de nodos: minusculas, sin espacios ni puntos; conteo antes y despues), R10 (todo modelo contra linea base de popularidad).
3. Leiden no es reproducible con nodos str: documenta el tratamiento (semilla, nodos enteros) y la variacion entre corridas.
4. Si una regla ya se cumple, documentalo con la celda; no cambies lo que esta bien.
5. Restart & Run All completo. Copia al informe SOLO cifras de esas salidas.
6. Entrega: `fases_pi/faseB_cifras.json`, `fases_pi/faseB_correcciones.md` (comentario/regla -> que cambio -> antes -> despues), `fases_pi/faseB_figura.png` (proyeccion o curva de robustez).
7. Cifras minimas: nodos, aristas y densidad tras normalizar; top-3 por centralidad; puntos de articulacion; robustez; modularidad y n.o de comunidades por algoritmo (Girvan-Newman, Louvain, Leiden, etiquetas); hit@k vs popularidad sin fuga; organizaciones puente con evidencia.
8. Si algo bloquea, detente y reporta. No hagas git add/commit/push.
