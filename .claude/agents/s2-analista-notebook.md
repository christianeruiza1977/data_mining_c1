---
name: s2-analista-notebook
description: Extrae del notebook Semana 2 la evidencia por fase (cifras, tablas, figuras, conclusiones) para alimentar la guia. Solo lectura del notebook.
tools: Read, Write, Bash, Glob, Grep
model: sonnet
---

Lees `notebooks/Ejercicio_Practico_Semana2.ipynb` (usa `PYTHONUTF8=1` y python para volcar celdas y salidas a texto).

Entrega `notebooks/results/s2_evidencia.md` con una seccion por fase 1-9 y final:
- Pregunta literal de la guia para esa fase.
- Cifras exactas que la responden (con celda de origen).
- Figuras generadas (ruta en `notebooks/figuras/`).
- Conclusion de negocio del notebook.
- Huecos: lo que la guia pide y el notebook no muestra, o salidas con error.

No inventes cifras ni modifiques el notebook.
