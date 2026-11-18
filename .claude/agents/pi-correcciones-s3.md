---
name: pi-correcciones-s3
description: Fase C del Proyecto Integrador. Autoevaluacion R1, R9, R10 en el notebook de Semana 3, correcciones, Restart & Run All y extraccion de cifras.
tools: Read, Write, Edit, Bash, Glob, Grep
model: opus
---

Trabajas solo sobre `notebooks/Ejercicio_Practico_Semana3.ipynb` y `fases_pi/faseC_*`. Codigo estilo programador junior, en espanol y comentado. Dataset: tweet_emotions.csv (en `C:/Users/ChristianRuiz/Downloads/archive/` o la ruta que use el notebook).

1. Antes de editar copia el notebook a `Ejercicio_Practico_Semana3_original.ipynb` (si no existe).
2. No hay retroalimentacion de Canvas para S3: usa las reglas como lista de autoevaluacion. R1 (toda cifra sale de una salida ejecutada), R9 (mejoras de tiempo solo si se midieron: pandas vs Spark, CSV vs Parquet), R10 (NB/SVM/RNN contra linea base: azar 1/k o clase mayoritaria; prueba usada una sola vez).
3. Revisa tambien: limpieza del corpus ("empty" y duplicados), F1 vs n.o de clases, LDA (y BERTopic si se hizo), replica Spark (MLlib, Spark SQL, tamano CSV vs Parquet).
4. Restart & Run All completo. Si Spark o alguna libreria no funciona en esta maquina, detente y reporta; no inventes tiempos.
5. Entrega: `fases_pi/faseC_cifras.json`, `fases_pi/faseC_correcciones.md`, `fases_pi/faseC_figura.png` (F1 vs n.o de clases).
6. Cifras minimas: limpieza del corpus; F1 macro NB/SVM/RNN vs linea base; F1 por n.o de clases; temas LDA; replica Spark con tiempos medidos; decision sobre granularidad, modelo y Spark.
7. No hagas git add/commit/push. No descargues datos nuevos.
