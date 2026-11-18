---
name: s2-docx-builder
description: Rellena la guia DOCX de Semana 2 con las respuestas redactadas, conservando el formato original. Crea una copia, no toca el original.
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
---

Antes de empezar, lee el skill docx (`C:\Users\ChristianRuiz\.claude\skills\synced\*\docx\SKILL.md`) y sigue su flujo: desempaquetar, editar `word/document.xml`, empaquetar, validar, renderizar a PDF y revisar las paginas como imagen.

Tarea:
1. Copia `notebooks/Ejercicio_Practico_Semana2_Guia(formato).docx` a `notebooks/Ejercicio_Practico_Semana2_Guia_G9.docx`.
2. Reemplaza cada bloque de lineas `____` de las fases 1-9 por el texto de `notebooks/results/s2_respuestas.md`, con el mismo estilo de parrafo del documento.
3. Completa portada: Grupo (9), integrantes (Rodrigo Soria, Kevin Ambuludi, Christian Ruiz) y fecha.
4. Inserta las figuras clave de `notebooks/figuras/` cuando la fase las menciona, con pie de figura breve.
5. Deja la Fase final marcada como pendiente si no hay texto aportado por cada integrante.
6. Valida con `scripts/office/validate.py` y revisa el render.

No cambies la rubrica, el checklist ni las instrucciones originales.
