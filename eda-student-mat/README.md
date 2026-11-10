# Análisis exploratorio de datos: rendimiento en matemáticas (student-mat)

Análisis exploratorio (EDA) del dataset **Student Performance**, asignatura de matemáticas. Sigue los 12 pasos de
la plantilla `11-eda_template` del curso, con explicaciones en español: qué se hace en cada paso, por qué y qué
se encontró.

**Autor:** Felipe Giraldo Zapata

---

## Contenido del repositorio

```text
eda-student-mat/
├── EDA_student_mat.ipynb     # el análisis, con las salidas de cada celda
├── data/
│   └── raw/
│       └── student-mat.csv   # datos originales (no se modifican)
├── requirements.txt          # paquetes necesarios
├── .gitignore                # archivos que Git no debe subir
└── README.md                 # este archivo
```

## Los datos

| Elemento | Detalle |
|---|---|
| Fuente | [UCI Machine Learning Repository: Student Performance](https://archive.ics.uci.edu/dataset/320/student+performance) (Cortez y Silva, 2008) |
| Una fila | Un estudiante de secundaria de uno de dos colegios públicos de Portugal |
| Tamaño | 395 filas y 33 columnas |
| Variable objetivo | `G3`, la nota final de matemáticas (0 a 20); se aprueba con 10 o más |

## Requisitos previos e instalación

Hay dos formas de preparar el entorno: con **pip** (pasos 3 a 5) o con **conda** (más abajo). Usa solo una de las
dos.

### 1. Requisitos previos

- **Python 3.11 o superior.** El análisis se probó con Python 3.13.
- **VS Code** con la extensión **Jupyter**, o Jupyter Notebook / JupyterLab.
- **Git**, para clonar el repositorio.

### 2. Clonar el repositorio

```bash
git clone https://github.com/xXMeisnerXx/eda-student-mat.git
cd eda-student-mat
```

### 3. Crear un entorno virtual

El entorno virtual (`.venv`) guarda los paquetes de este proyecto separados del resto del computador:

```bash
python -m venv .venv
```

Actívalo según tu sistema:

```bash
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS / Linux
```

### 4. Instalar los paquetes

Con el entorno activado, instala los cinco paquetes básicos:

```bash
pip install jupyter pandas matplotlib seaborn plotly
```

O, de forma equivalente, desde el archivo de requisitos:

```bash
pip install -r requirements.txt
```

| Paquete | Para qué se usa | Versión probada |
|---|---|---|
| `jupyter` | Abrir y ejecutar el notebook | IPython 9.18, ipykernel 7.4 |
| `pandas` | Cargar y analizar la tabla de datos | 3.0.5 |
| `matplotlib` | Base de los gráficos: títulos, ejes, leyendas | 3.11.2 |
| `seaborn` | Histogramas, boxplots, barras, dispersión y mapa de calor | 0.13.2 |
| `plotly` | Gráfico interactivo del paso 10 | 7.1.0 |

### 5. Comprobar la instalación

```bash
python -c "import pandas, matplotlib, seaborn, plotly; print('Paquetes instalados')"
```

Si aparece `Paquetes instalados`, todo está listo. Si aparece `ModuleNotFoundError`, revisa que el entorno esté
activado y repite el paso 4.

### Alternativa: instalación con conda

Si usas Anaconda o Miniconda, reemplaza los pasos 3 y 4 por estos comandos, desde la carpeta del repositorio:

```bash
conda create -n eda-student-mat python=3.12
conda activate eda-student-mat
conda install -c conda-forge jupyter pandas matplotlib seaborn plotly
```

- `conda create` crea un entorno llamado `eda-student-mat` con Python 3.12.
- `conda activate` lo activa; hay que activarlo cada vez que abras una terminal nueva.
- `conda install -c conda-forge` instala los cinco paquetes desde el canal `conda-forge`.

Después, comprueba la instalación con el mismo comando del paso 5. En VS Code, elige `eda-student-mat` como kernel
en lugar de `.venv`.

## Carga de paquetes

La primera celda de código del notebook carga los paquetes con sus alias habituales y fija el estilo de los
gráficos:

```python
import matplotlib.pyplot as plt
import pandas as pd
import plotly.express as px
import seaborn as sns

sns.set_theme(style="whitegrid")
```

Para ver qué versiones quedaron instaladas, puedes ejecutar en una celda:

```python
import matplotlib
import plotly

print("pandas:    ", pd.__version__)
print("matplotlib:", matplotlib.__version__)
print("seaborn:   ", sns.__version__)
print("plotly:    ", plotly.__version__)
```

## Cómo ejecutar el análisis

1. Abre la carpeta `eda-student-mat` en VS Code (**File → Open Folder**).
2. Abre `EDA_student_mat.ipynb` y elige el entorno `.venv` como kernel, arriba a la derecha.
3. Ejecuta todas las celdas en orden con **Run All**.

También puedes abrirlo desde la terminal con `jupyter notebook EDA_student_mat.ipynb`.

El notebook lee los datos con la ruta `data/raw/student-mat.csv`. Por eso hay que abrirlo desde la carpeta
principal del repositorio; si lo mueves de lugar, la ruta deja de funcionar.

> **Sobre plotly:** GitHub no muestra los gráficos de plotly. Al ver el notebook en GitHub, la celda del gráfico
> interactivo aparece vacía; para verlo, ejecuta el notebook en Jupyter o en VS Code. Los demás gráficos sí se ven
> en GitHub.

## Estructura del análisis

| Paso | Contenido |
|---|---|
| 1. Entender el dataset | Fuente, qué es una fila, grupos de columnas, variable objetivo |
| 2. Cargar los datos | `raw` intacto y copia de trabajo `df` |
| 3. Primera mirada | Tamaño, primeras y últimas filas, tipos de columna |
| 4. Calidad de los datos | Faltantes, duplicados y faltantes escondidos (notas en 0) |
| 5. Estadística descriptiva | Columnas numéricas y de texto |
| 6. Una columna a la vez | Histograma, boxplot, participación de categorías, prueba de categorías repetidas |
| 7. Valores atípicos | Regla del IQR en 6 columnas |
| 8. Variable objetivo | Distribución de `G3` y tasa de aprobación |
| 9. Dos columnas a la vez | `G2` y `G3`, `failures` y `G3`, `higher` y aprobación |
| 10. Correlaciones | Mapa de calor y gráfico interactivo con plotly |
| 11. Hallazgos | Tabla de 10 hallazgos con su evidencia |
| 12. Próximos pasos | Limpieza pendiente y preguntas para después |

## Hallazgos principales

- **38 estudiantes (9.6 %) tienen nota final 0**, y los 38 tienen 0 ausencias. Lo más probable es que no
  presentaran la evaluación final: son valores faltantes escondidos.
- **67.1 % de los estudiantes aprueba** (nota final de 10 o más).
- **A más materias perdidas antes, menor nota final:** la mediana baja de 11 (ninguna materia perdida) a 7 (3
  materias).
- **Las notas del año dicen casi lo mismo:** la correlación entre la nota del segundo periodo y la final es 0.90.
- **Los ceros esconden una relación:** la correlación entre ausencias y nota final es 0.03 con todos los
  estudiantes y −0.21 sin los ceros.

## Fuente

P. Cortez y A. Silva (2008). *Using Data Mining to Predict Secondary School Student Performance*. Proceedings of
the 5th Future Business Technology Conference (FUBUTEC 2008), pp. 5 a 12, Porto, Portugal. Datos disponibles en el
[UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/320/student+performance).
