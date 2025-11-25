Carpeta de Drive donde se encuentran los archivos generados:

https://drive.google.com/drive/u/1/folders/1WMsXQBOuRNs6npcmxLrUSFgTLEXzBXJe

# Preprocesamiento de datos con fastp para archivos comprimidos

fastp -i CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz -I CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz -o R1_trimmed.fq.gz -O R2_trimmed.fq.gz --html fastp_report.html

- Fastp recorta los adaptadores y elimina las lecturas de baja calidad.
- El informe generado (fastp_report.html) da información sobre la calidad de las lecturas antes y después del recorte.
- Los archivos de salida (R1_trimmed.fq.gz y R2_trimmed.fq.gz) serán los archivos de lecturas recortadas y listas para el siguiente paso.

# Ensamblaje de metagenomas

megahit -1 R1_trimmed.fq.gz -2 R2_trimmed.fq.gz -o megahit_output

- MEGAHIT genera un conjunto de contigs ensamblados dentro de la carpeta megahit_output

# Binning (Agrupamiento de contigs)

metabat2 -i megahit_output/final.contigs.fa -o bins/bin

- Agrupa los contigs basándose en la co-abundancia y patrones de frecuencia de nucleótidos. Es uno de los métodos más utilizados para obtener MAGs (genomas metagenómicos agregados).
- MetaBAT2 generará archivos de binning en el directorio bins/, donde cada archivo de bin representará un MAG.

# Evaluación de calidad de los ensamblajes y MAGs


- QUAST para evaluar la calidad global del ensamblaje, utiliza el archivo de contigs generado por MEGAHIT
- Genera un informe con métricas de calidad del ensamblaje, que ayuda a entender cómo de bien se realizó el ensamblaje.
- BUSCO para evaluar la completitud y contaminación de los MAGs, comparando los contigs de cada bin con un conjunto de genes conservados específicos del dominio de estudio.
- Genera un informe con los resultados de la evaluación, proporcionando métricas sobre la completitud y contaminación de los MAGs.
  
** ESTA PARTE AUN POR COMPLETAR PORQUE LA VERSION DE PYTHON QUE TENGO NO ES COMPATIBLE CON ESTOS DOS PROGRAMAS.


