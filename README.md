# Actividad-Grupo-5-UNIR-IPC

Repositorio de Bioinformática para la actividad grupal de la asignatura Introducción a la Programación Científica del Máster en Bioinformática por la UNIR. (2026)

## Estructura del repositorio

| Carpeta / fichero | Para qué sirve |
|---|---|
| `data/` | Datos de entrada y ficheros intermedios o finales de cada fase (FASTQ, FASTA, BAM, VCF). |
| `scripts/` | Scripts en Python de cada fase del flujo de trabajo. |
| `result_fastqc/` | Informes de control de calidad generados por FastQC. |
| `result_visualizacion/` | Gráficas finales del análisis. |
| `README.md` | Descripción del proyecto y del flujo de trabajo. |

## Flujo de trabajo para el análisis de datos de secuenciación

### 1. Control de calidad de las secuencias (FastQC)

En esta primera fase se plantea la evaluación de la calidad de los datos de secuenciación mediante la herramienta FastQC, con el objetivo de identificar anomalías en las lecturas antes de realizar los análisis posteriores.

#### Archivos de entrada

Los datos de entrada corresponden a archivos en formato fastq, almacenados en el directorio `data/`.

#### Procedimiento

Se utiliza un script desarrollado en Python (`scripts/control_calidad_fastqc.py`) para automatizar la ejecución de FastQC y almacenar los informes generados. Los resultados se almacenan en el directorio `result_fastqc/`.

### 2. Filtrado de secuencias (fastp)

Tras evaluar la calidad inicial, se procede a la limpieza de las lecturas. En esta etapa se eliminan los adaptadores y se filtran aquellas secuencias de baja calidad que no cumplan con los umbrales mínimos, utilizando la herramienta `fastp`.

#### Archivos de entrada

- Archivos FASTQ originales analizados en el paso anterior (`data/`).

#### Procedimiento

Mediante el script de Python `scripts/fastp.py`, se automatiza la ejecución de fastp sobre los datos crudos para realizar el recorte (trimming) y filtrado.

#### Resultados

- Lecturas procesadas y limpias (guardadas como `data/procesados.fasta` o archivos fastq filtrados), listas para ser ensambladas.

### 3. Ensamblado

Ensamblado de las lecturas procesadas frente a la secuencia de referencia.

#### Archivos de entrada

- `data/referencia.fasta`: secuencia de referencia.
- `data/procesados.fasta`: lecturas procesadas en la fase anterior.

#### Procedimiento

El script `scripts/ensamblado.py` realiza el ensamblado (mapeo) de las lecturas.

#### Resultados

- `data/ensamblado.bam`: alineamiento resultante en formato BAM.

### 4. Evaluación del ensamblaje 

En esta fase planteamos la evaluación de la calidad del ensamblaje, mediante la herramienta BUSCO (Benchmarking Universal Single-Copy Orthologs), para comprobar que el ensamblaje está completo buscando genes que normalmente están presentes en organismos del mismo grupo.

#### Archivos de entrada

- `data/referencia.fasta`: secuencia de referencia.
- `data/procesados.fasta`: secuencias procesadas en la fase de filtrado.
- `data/ensamblado.bam`: archivo de alineamiento obtenido en la fase anterior.

#### Procedimiento

Utilizamos el script de Python `scripts/busco.py` para realizar el análisis de calidad del ensamblaje mediante BUSCO. Esta herramienta comprueba la presencia de determinados genes y los clasifica en las siguientes categorías:

- Completos (C): genes que se han encontrado completamente.
- Completos de copia única (S): genes completos que aparecen una sola vez.
- Completos duplicados (D): genes completos que aparecen más de una vez.
- Fragmentados (F): genes que se han encontrado de forma incompleta.
- Ausentes (M): genes que no se han encontradi en el ensamblaje.

Con esta información comprobamos si el ensamblaje realizado tiene una buena calidad o si faltan partes importantes de la secuencia.

#### Resultados

Guardamos los resultados del análisis en la carpeta `result_busco/` y creamos el archivo `result_busco/busco.txt` para recoger la información obtenida mediante el script.
Después de realizar los análisis podemos consultar los porcentajes de genes completos, fragmentados y ausentes, para poder valorar si el ensamblaje es suficientemente completo para continuar con las siguientes fases del proyecto

### 5. Anotación de genomas (Prokka)

Una vez ensamblado el genoma , se procede a la predicción y anotación funcional de genes, regiones codificantes y otras características genómicas utilizando `Prokka`.

#### Archivos de entrada

- Secuencia ensamblada (FASTA del ensamblaje generado en la fase 3).

#### Procedimiento

Un script orquestador en Python (`scripts/anotacion_prokka.py`) ejecuta Prokka sobre el ensamblado para localizar e identificar funcionalmente las regiones genómicas de interés.

#### Resultados

- Fichero de variantes (anotaciones.gbk), almacenados en la carpeta `data/` o en su propio subdirectorio de resultados.

### 6. Visualización de resultados

Fase final del flujo de trabajo donde se generan las representaciones gráficas para interpretar los datos biológicos y estadísticos extraídos a lo largo de las distintas etapas.

#### Archivos de entrada

- Informes y datos estadísticos generados en las fases previas (resultados de calidad, estadísticas del BAM o los datos de la anotación de Prokka).

#### Procedimiento

Se ejecuta el script principal `scripts/visualizacion.py`. Este script parsea los datos tabulares o reportes previos y utiliza librerías de Python  para componer figuras comparativas y de resumen.

#### Resultados

- Gráficas finales (en formato PNG) almacenadas en el directorio `result_visualizacion/`.
