# Actividad-Grupo-5-UNIR-IPC

Repositorio de Bioinformática para la actividad grupal de la asignatura Introducción a la Programación Científica del Máster en Bioinformática por la UNIR. (2026)

## Estructura del repositorio

| Carpeta / fichero | Para qué sirve |
|---|---|
| `data/` | Datos de entrada y ficheros intermedios o finales de cada fase (FASTQ, FASTA, BAM, VCF). |
| `scripts/` | Scripts en Python de cada fase del flujo de trabajo. |
| `result_fastqc/` | Informes de control de calidad generados por FastQC. |
| `result_busco/` | Resultados de la evaluación de completitud con BUSCO. |
| `result_visualizacion/` | Gráficas finales del análisis. |
| `README.md` | Descripción del proyecto y del flujo de trabajo. |
## Flujo de trabajo para el análisis de datos de secuenciación

### 1. Control de calidad de las secuencias (FastQC)

En esta primera fase se plantea la evaluación de la calidad de los datos de secuenciación mediante la herramienta FastQC, con el objetivo de identificar anomalías en las lecturas antes de realizar los análisis posteriores.

#### Archivos de entrada

Los datos de entrada corresponden a archivos en formato fastq, almacenados en el directorio data/.

#### Procedimiento

Se utiliza un script desarrollado en Python (scripts/control_calidad_fastqc.py) para automatizar la ejecución de FastQC y almacenar los informes generados. Los resultados se almacenan en el directorio result_fastqc/.


### 3. Ensamblado

Ensamblado de las lecturas frente a la secuencia de referencia, con el script `scripts/ensamblado.py`.

#### Archivos de entrada

- `data/referencia.fasta`: secuencia de referencia.
- `data/procesados.fasta`: lecturas procesadas en la fase anterior.

#### Procedimiento

El script `scripts/ensamblado.py` realiza el ensamblado y genera como resultado `data/ensamblado.bam`.

#### Resultados

- `data/ensamblado.bam`: alineamiento resultante en formato BAM.




### filtrado de secuencias

### ensamblaje y evaluación

### anotación de genomas.
