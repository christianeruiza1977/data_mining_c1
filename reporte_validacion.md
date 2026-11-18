# Reporte de validacion independiente - Parte II (Grupo 9) - revalidacion tras correcciones

> Generado por el agente pi-validador; guardado por el orquestador (el entorno bloquea la escritura desde el subagente).
> Primera pasada: 1 CRITICAL, 9 WARNING, 7 SUGGESTION. Segunda pasada (esta): **0 CRITICAL, 2 WARNING, 8 SUGGESTION**.
> Correcciones posteriores a esta pasada: **W2 resuelto** (Fase C citada con la precision impresa por cada celda; barrido sin excesos de decimales; eliminada la frase sobre "happiness"), **S6 resuelto** (88 %), **N1 resuelto** (`faseA_correcciones.md` alineado con la celda 19). Queda **W9** (Edge et al. 2024 incompleta, completar a mano).

**Alcance:** extraccion desde cero de `grupo9_Proyecto_Integrador.docx` (regenerado), salidas actuales de `notebooks/Ejercicio_Practico_Semana{1,2,3}.ipynb` y `fases_pi/*`. Recalculo con codigo propio en `.venv312` y cache local de kagglehub; sin ejecutar notebooks. P*n*/T*n* = posicion del elemento en el cuerpo del docx (desde 0); celdas en indice base 0. Scripts: scratchpad `validador/v_s1.py`, `v_s2.py`, `v_s3.py`, `sweep.py`.

## Resumen

| Severidad | Cantidad |
|---|---|
| CRITICAL | 0 (antes 1) |
| WARNING | 2 (antes 9) |
| SUGGESTION | 8 (antes 7) |

## Tabla de recalculos (segunda pasada)

| # | Cifra | Informe | Salida actual del notebook | Recalculo propio | Resultado |
|---|---|---|---|---|---|
| 1 | S1: val / test que repiten un perfil de train | 88/132; 93/101 | celdas 7, 27 y 31 | 88/132; 93/101 | OK |
| 2 | S1: F1 en las 8 filas nuevas de test, IC Wilson | 0.4111; 4/8 [0.2152, 0.7848] | celda 27 | identico | OK |
| 3 | S1: F1 en las 44 filas nuevas de val | 0.2981; 14/44 [0.2000, 0.4656] | celda 31 | identico | OK |
| 4 | S1: mayoritaria (Happiness), test completo | 0.0380 | celda 27: 0.0380 (exactitud 0.1287) | 0.0380 | OK |
| 5 | S1: mayoritaria, 8 filas nuevas de test | 0.0444 | celda 27: 0.0444 | 0.0444 | OK |
| 6 | S1: mayoritaria, 44 filas nuevas de val | 0.0714 | celda 27: 0.0714 | 0.0714 | OK |
| 7 | S1: azar 1/6 | 0.1667 | celda 27: 0.1667 | 0.1667 | OK |
| 8 | S2: puntos de articulacion | apache, babel, facebook, google, microsoft, python, reactjs, vuejs | celda 20 | identicos | OK, sin cambios |
| 9 | S2: hit@1/3/5 sin fuga, n = 724 | co-ocurrencia 0.837/0.908/0.929; popularidad 0.786/0.815/0.871 | celda 29 | identicos | OK, sin cambios |
| 10 | S2: sin el bloque azure + microsoft, n = 179 | 0.337/0.625/0.713; 0.131/0.249/0.478 | celda 29 | identicos | OK, sin cambios |
| 11 | S3: SVM k = 3 | 0.5427 | celda 85 | 0.5427 +- 0.0945 | OK, sin cambios |
| 12 | S3: SVM k = 7 | 0.3155 +- 0.0533; 2.21 veces el azar | celda 85 | identico | OK, sin cambios |
| 13 | S3: SVM 13 clases y mayoritaria | 0.1976; 0.0272 | celdas 60 y 56 (0.027) | 0.1976; 0.0272 | OK |

Las salidas de S2 y S3 son identicas a las de la primera validacion; S3 solo cambio el markdown de las celdas 95 y 129.

## Tiempos de S1 frente a las salidas actuales

| Informe | Salida actual | Resultado |
|---|---|---|
| T59 y T88 (despues): Grid 1.37 s; Optuna 3.10 s | celda 26: "Tiempo: 1.37s" y "Tiempo: 3.10s" | OK |
| T88 (antes): Grid 1.22 s; Optuna 2.91 s | S1 original, celda 26 | OK |
| P100: Polars Parquet 0.0068 s vs pandas 0.0662 s, "unas 10 veces" | celda 37 (minimos 0.0068 y 0.0662; razon 9.7) | OK |
| Markdown celdas 28 y 38 | citan 2.71/1.18/1.37/3.10 y 0.0974/0.0662/0.0448/0.0068/0.0181/0.1562, igual que las salidas | OK |
| Tiempos viejos (1.81, 3.46, 3.36, 1.57, 0.0139, 0.2006) | no aparecen ni en el informe ni en el notebook S1 | OK |

S1 ejecutado en secuencia (execution_count 1 a 15) sin errores. P101 (Python 3.14.4) coincide con la celda 4.

## Estado de los hallazgos anteriores

| ID | Hallazgo | Estado | Evidencia |
|---|---|---|---|
| C1 | Fase A sin linea base trivial (R10) | **Resuelto** | Celda 27: `DummyClassifier(strategy='most_frequent')` ajustado solo con train, reutiliza `y_pred_test`. Cifras en T59, T93, T88 (fila R10, antes "No calculada"), P97 y P103. P94 ya no declara la limitacion. |
| W1 | 52.5 % fuera del notebook | **Resuelto en el informe** | P65 cita la celda 19 (3 grupos, 10.4 % ruido, silueta 0.192). Residuo en `fases_pi` (N1). |
| W2 | Fase C con mas decimales que la salida | **Pendiente** | Ver WARNING W2. |
| W3 | Cifras derivadas no impresas | **Pendiente, rebajado a SUGGESTION** | Ver SUGGESTION W3. |
| W4 | RDD 27.54/5.48 | **Resuelto** | P100: "27.5 s frente a 5.5 s", como la celda 103. |
| W5 | Figura 3 hecha fuera del notebook | **Resuelto (declarado)** | P83 aclara que es `fases_pi/faseC_figura.png` (tabla de la celda 85) y que la del notebook esta en la celda 87. Detalle de formato en S8. |
| W6 | 0.7826 presentado como "honesto" | **Resuelto** | P91: "CV agrupada optimista"; resultado honesto en filas nuevas (0.2981 / 0.4111). T93 y T59 actualizados. |
| W7 | Brecha de genero con val memorizada | **Resuelto** | T59, P64 y P99 advierten 88 de 132 filas memorizadas y 0 casos de Sadness en hombres en filas nuevas. |
| W8 | "5 a 7 categorias" | **Resuelto** | T79, P84, P101, P105 dicen 7; celdas 95 y 129 de S3 tambien. "5 a 7" solo queda como "antes" en `fases_pi/faseC_correcciones.md` (correcto). |
| W9 | Referencias | **Parcial** | Rousseeuw (P119) corregido a *Journal of Computational and Applied Mathematics*. Edge et al. (P110) sin cambios. |
| S1 | "Polars leyo" | **Resuelto** | P100: "leyo y agrego". |
| S2 | Datasets sin cita APA en el texto | Pendiente | P52-P54 siguen con "(Kaggle, emirhanai)". |
| S3 | Akiba unico autor; lecturas sin URL | Pendiente | P108, P116, P127 sin cambios. |
| S4 | 0.9593 sin salvedad | **Resuelto** | Las cuatro apariciones (P57, T59, T88, P103) llevan "(inflado: 93/101 vistas)". |
| S5 | Fuente del "antes" de la Fase C | **Resuelto** | P89 cita `notebooks/anteriores/Ejercicio_Practico_Semana3_G9.ipynb`. |
| S6 | "88.0 %" vs "88 %" | Pendiente | T79 sin cambios. |
| S7 | Nota de que los AP no cambian | Pendiente (opcional) | Con todos los miembros normalizados: 87 aristas y los mismos 8 AP. |

## WARNING

### W2 (pendiente). Fase C: cifras con mas precision que la salida impresa (R1)

| Cifra | Donde aparece | Lo que imprime el notebook |
|---|---|---|
| 0.0769 | T79, T93, P97 | celda 56: "~0.077" |
| 0.0272 | T79, T93, P97 | celda 56: "0.027" |
| 2.07 % | P54, T79 | celda 14: "2.1" |
| 0.3002 / 0.2291 | T79 | celda 138: "0.3" / "0.229" |
| 0.1763 | T79 | celda 142: "0.176" |
| 0.1665 -> 0.1422 | T79 | celda 148: "0.166" / "0.142" |
| 88.0 % | T79 | celda 142: "88 %" |
| "la clase que mas aporta es happiness" | P84 | ninguna salida; solo `results/fase9.json` y `faseC_cifras.json` |

Valores confirmados por recalculo donde se replicaron. El riesgo es de trazabilidad: `notebooks/results/*.json` son de la corrida del 17-nov. Opciones: citar la precision impresa, o imprimir 4 decimales en las celdas 14, 56, 138, 142 y 148 y re-ejecutar.

### W9 (parcial). Edge et al. (2024) incompleta

P110: "Edge et al. (2024). From local to global: A Graph RAG approach." Faltan autores, titulo completo y fuente (arXiv o URL). Se cita en P49.

## SUGGESTION

- **W3 (rebajado).** Cifras derivadas no impresas: 10 598 filas (P53, T69), 45.9 % (T59, P64, P103), 19.7 % (P64), 4 748 tweets (T79), densidad 0.1024 (T69, declarada "calculada"). Recalculo las confirma (10 598; 0.4589; 182/924 = 0.197; 84/820 = 0.1024). Ideal: imprimirlas en sus celdas.
- **S2.** Citar los datasets en APA en el texto, p. ej. "(emirhanai, s. f.)", en P52-P54.
- **S3.** Optuna atribuido solo a "Akiba, T."; usar "Optuna Contributors" o Akiba et al. (2019). Lecturas de tendencia (P116, P127) sin URL ni plataforma.
- **S6.** "88.0 %" -> "88 %" en T79.
- **S7.** Opcional: anotar en T88 (R8) que los 8 AP no cambian aunque se normalicen tambien los miembros.
- **S8 (nuevo).** `fases_pi/faseC_figura.png` usa coma decimal ("minimo 0,30", "SVM 0,23"); el informe usa punto.
- **N1 (nuevo).** `fases_pi/faseA_correcciones.md` (lineas 17 y 34) sigue citando el "52.5 %"; alinear con la celda 19.
- **N2 (nuevo, proceso).** I.6 de la plantilla: "Los notebooks no se vuelven a subir". Las cifras corregidas solo existen en los notebooks locales; conviene confirmar con el docente como se evidencian (anexo, enlace o subida opcional).

## Contradicciones nuevas

Ninguna. Todas las apariciones de 0.9593 llevan la salvedad; "5 a 7" no aparece en informe ni notebooks; P103-P105 coinciden con T59, T79 y T93; P97 y P103 coinciden con la celda 27; P101 coincide con la celda 4 de S1 y con S2/S3.

## Rubrica I.7 / I.8

- Cada fase con tabla, figura y decision: A (T59, Fig. 1, P64), B (T69, Fig. 2, P74/P75), C (T79, Fig. 3, P84).
- Seccion 5 (T88): antes -> despues, 17 filas (se anadio R10 de la Fase A).
- Fase D: lecciones con al menos dos fases cada una.
- Sin corchetes guia ni `____` en la Parte II. Anexo de roles correcto.
- Parte III identica a la plantilla y vacia (correcto en esta etapa).
