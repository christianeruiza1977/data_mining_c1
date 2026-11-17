# ProjectDataMiningII

Plantilla para desarrollar proyectos de ciencia de datos y machine learning con
Python. Mantén los datos originales sin modificar y guarda los resultados
generados fuera del código fuente.

## Estructura

```text
ProjectDataMiningII/
├── configs/                  # Configuración del proyecto
├── data/
│   ├── external/             # Datos de fuentes externas
│   ├── interim/              # Datos intermedios
│   ├── processed/            # Datos limpios, listos para análisis/modelado
│   └── raw/                  # Datos originales; no modificarlos
├── docs/                     # Documentación y diccionario de datos
├── notebooks/                # Exploración y experimentos
├── outputs/
│   ├── figures/              # Gráficos exportados
│   ├── models/               # Modelos entrenados
│   ├── predictions/          # Predicciones
│   └── reports/              # Informes y métricas
├── scripts/                  # Comandos ejecutables del proyecto
├── src/project_data_mining_ii/
│   ├── data/                 # Carga, validación y preparación de datos
│   ├── features/             # Ingeniería de características
│   ├── models/               # Entrenamiento, evaluación e inferencia
│   ├── utils/                # Utilidades compartidas
│   └── visualization/        # Funciones de visualización
├── tests/
│   ├── integration/          # Pruebas entre componentes
│   └── unit/                 # Pruebas unitarias
├── logs/                     # Registros de ejecución
├── .env.example              # Variables de entorno de ejemplo
├── .gitignore
├── pyproject.toml            # Configuración de herramientas
├── requirements-dev.txt      # Dependencias para desarrollo
└── requirements.txt          # Dependencias principales
```

## Configuración

Requiere Python 3.10 o posterior. Desde la raíz del proyecto:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements-dev.txt
```

En Linux o macOS, activa el entorno con `source .venv/bin/activate`.

Si el proyecto necesita credenciales o parámetros locales, copia `.env.example`
a `.env` y completa los valores. No guardes secretos ni datos sensibles en Git.
Los directorios de datos y resultados se mantienen en el repositorio mediante
archivos `.gitkeep`; sus contenidos se ignoran.

## Flujo de trabajo sugerido

1. Documenta el objetivo, las fuentes y las métricas en `docs/`.
2. Guarda los datos originales en `data/raw/` y explóralos en `notebooks/`.
3. Implementa transformaciones reutilizables en `src/project_data_mining_ii/`.
4. Guarda las salidas intermedias y preparadas en `data/interim/` y
   `data/processed/`.
5. Añade pruebas en `tests/` y comandos reproducibles en `scripts/`.
6. Guarda modelos, predicciones, gráficos e informes en `outputs/`.

Ejecuta las pruebas desde la raíz:

```powershell
python -m pytest
```

