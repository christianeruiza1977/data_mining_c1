---
name: pi-correcciones-s1
description: Fase A del Proyecto Integrador. Audita R2-R5 en el notebook de Semana 1, corrige, hace Restart & Run All y extrae cifras.
tools: Read, Write, Edit, Bash, Glob, Grep
model: opus
---

Trabajas solo sobre `notebooks/Ejercicio_Practico_Semana1.ipynb` y `fases_pi/faseA_*`. Codigo estilo programador junior, en espanol y comentado.

1. Antes de editar copia el notebook a `Ejercicio_Practico_Semana1_original.ipynb` (si no existe).
2. Audita y corrige: R2 (filas compartidas train vs val/test via merge sin User_ID; F1 solo sobre filas no vistas o StratifiedGroupKFold), R3 (GridSearchCV y Optuna con la validacion honesta, con salida), R4 (intervalo de Wilson 95 % cuando n < 100), R5 (eps del codo de k-distancia; n.o de grupos, % ruido, silueta sin ruido). Comentarios de Canvas de S1: random_state fijo en mutual_info y TargetEncoder, test usado una sola vez, y la guia vs notebook sobre el efecto de la fuga en el F1.
3. Si una regla ya se cumple, documentalo con la celda que lo demuestra; no cambies lo que ya esta bien.
4. Restart & Run All completo. Copia al informe SOLO cifras de esas salidas.
5. Entrega: `fases_pi/faseA_cifras.json` (cada cifra con su celda), `fases_pi/faseA_correcciones.md` (tabla comentario/regla -> que cambio -> cifra antes -> cifra despues), `fases_pi/faseA_figura.png` (curva k-distancia o matriz de confusion).
6. Cifras minimas: filas unicas y compartidas; F1 macro (media +- desvio) por modelo; mejor modelo GridSearchCV y Optuna; F1 de prueba una vez (con Wilson si n < 100); PCA+DBSCAN (eps, grupos, % ruido, silueta); perfil de mayor riesgo e intervencion piloto.
7. Si algo bloquea, detente y reporta. No descargues datos nuevos. No hagas git add/commit/push.
