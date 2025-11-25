# Caracterización del microbioma respiratorio

Este repositorio contiene un pipeline bioinformático para el análisis de la composición taxonómica del microbioma respiratorio, con un enfoque en la identificación de patógenos bacterianos en muestras metagenómicas respiratorias. El pipeline se basa en el repositorio clonado de nf-core/mag, el cual es un flujo de trabajo robusto para la ensamblaje y caracterización taxonómica de metagenomas, y se adapta para su uso con muestras respiratorias de tipo Illumina.

## Hipótesis

Se postula que la composición del microbioma respiratorio en pacientes con infecciones respiratorias muestra una prevalencia mayor de especies bacterianas patógenas, las cuales pueden desempeñar un papel crucial en el desarrollo o exacerbación de estas infecciones. A través de un análisis metagenómico exhaustivo, se espera identificar y caracterizar los patógenos respiratorios presentes, proporcionando una mejor comprensión de la interacción entre el microbioma y las enfermedades respiratorias.

## Objetivos

## Objetivo General: 

Aplicar un pipeline de análisis metagenómico para caracterizar la composición taxonómica del microbioma respiratorio y identificar especies bacterianas patógenas asociadas con infecciones respiratorias.

## Objetivos Específicos:

1. Ejecutar el pipeline nf-core/mag para la caracterización taxonómica de microbiomas respiratorios utilizando datos metagenómicos de tipo Illumina.

2. Identificar y clasificar especies bacterianas patógenas presentes en las muestras respiratorias, con énfasis en aquellas asociadas a infecciones respiratorias.

## Muestras y Datos

Se utilizará 1 muestras metagenómicas de tipo Illumina, correspondientes a pacientes diagnosticados con infecciones respiratorias. Estas muestras están disponibles en el repositorio de datos (...), y se analizarán utilizando el pipeline nf-core/mag para identificar las especies bacterianas presentes.

## Método

El pipeline utilizado en este proyecto es el flujo de trabajo nf-core/mag clonado, el cual es ampliamente utilizado para la caracterización metagenómica de microbiomas a partir de secuencias de datos de Illumina. Este pipeline ofrece un análisis eficiente, modular y reproducible de metagenomas, utilizando herramientas de última generación para cada paso del proceso. El análisis comprende los pasos siguientes:

1. Preprocesamiento de datos:

Lecturas cortas (Illumina): Se utiliza fastp para recortar adaptadores, Bowtie2 para eliminar lecturas contaminantes, y FastQC para control de calidad.
Lecturas largas (Nanopore): Se emplean porechop para recorte de adaptadores, NanoLyse para eliminar contaminaciones, y Filtlong para filtrado de calidad, con NanoPlot para visualización.

2. Ensamblaje de metagenomas:

MEGAHIT o SPAdes para ensamblaje de lecturas cortas, y hybridSPAdes para ensamblaje híbrido si se usan lecturas cortas y largas. Se puede hacer ensamblaje individual por muestra o co-ensamblaje usando información grupal.

3. Binning (Agrupamiento de contigs):

MetaBAT2 agrupa contigs en MAGs basándose en frecuencia de nucleótidos y patrones de co-abundancia. Se estima la abundancia de los MAGs según las profundidades de secuenciación.

4. Evaluación de calidad:

QUAST para evaluar la calidad del ensamblaje, y BUSCO para estimar la completitud y contaminación de los MAGs.

5. Clasificación taxonómica:

Los MAGs son clasificados con GTDB-TK (para MAGs de alta calidad) o CAT/BAT (para cualquier MAG).

6. Control de calidad:

Kraken2 o Centrifuge para clasificación de lecturas y detección de contaminaciones, visualizado con Krona. MultiQC genera un informe agregando resultados de calidad de todas las muestras.

## Resultados Esperados

Se espera que el análisis del microbioma respiratorio revele la presencia y abundancia de diversas especies bacterianas, incluidas aquellas patógenas, y que se puedan identificar patrones en la composición del microbioma en relación con las infecciones respiratorias.
