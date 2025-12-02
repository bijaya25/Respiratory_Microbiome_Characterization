Se ejecutan los siguientes comandos en Visual Studio Code, secuencialmente as►1:

# conda install -c bioconda fastp

En carpeta Dataset dentro de la carpeta de proyecto microbioma en Visual Studio Code

# fastp -i CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz -I CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz -o R1_trimmed.fq.gz -O R2_trimmed.fq.gz --html fastp_report.html

Se generaron los archivos:  
-rw-rw-r--   1 bijaya bijaya 2.2G Nov 25 16:05 R1_trimmed.fq.gz    **  
-rw-rw-r--   1 bijaya bijaya 2.2G Nov 25 16:05 R2_trimmed.fq.gz    **  
-rw-rw-r--   1 bijaya bijaya 135K Nov 25 16:05 fastp.json  
-rw-rw-r--   1 bijaya bijaya 477K Nov 25 16:05 fastp_report.html   **

# conda install -c bioconda megahit

En Dataset

# megahit -1 R1_trimmed.fq.gz -2 R2_trimmed.fq.gz -o megahit_output

Se genera la carpeta megahit_output con los archivos:  
-rw-rw-r-- 1 bijaya bijaya  262 Nov 25 16:54 checkpoints.txt  
-rw-rw-r-- 1 bijaya bijaya    0 Nov 25 16:54 done  
-rw-rw-r-- 1 bijaya bijaya 319M Nov 25 16:54 final.contigs.fa       **  
drwxrwxr-x 2 bijaya bijaya 4.0K Nov 25 16:54 intermediate_contigs  
-rw-rw-r-- 1 bijaya bijaya 161K Nov 25 16:54 log  
-rw-rw-r-- 1 bijaya bijaya  993 Nov 25 16:19 options.json  

# conda install -c bioconda metabat2

En Dataset

# metabat2 -i megahit_output/final.contigs.fa -o bins/bin

MetaBAT2 generará archivos de binning en el directorio bins/, donde cada archivo de bin representará un MAG:  
total 112M  
-rw-rw-r-- 1 bijaya bijaya 3.6M Nov 25 17:03 bin.10.fa  
-rw-rw-r-- 1 bijaya bijaya 7.2M Nov 25 17:03 bin.11.fa  
-rw-rw-r-- 1 bijaya bijaya 2.0M Nov 25 17:03 bin.12.fa  
-rw-rw-r-- 1 bijaya bijaya 484K Nov 25 17:03 bin.13.fa  
-rw-rw-r-- 1 bijaya bijaya 1.9M Nov 25 17:03 bin.14.fa  
-rw-rw-r-- 1 bijaya bijaya 365K Nov 25 17:03 bin.15.fa  
-rw-rw-r-- 1 bijaya bijaya  51M Nov 25 17:03 bin.16.fa  
-rw-rw-r-- 1 bijaya bijaya 2.0M Nov 25 17:03 bin.17.fa  
-rw-rw-r-- 1 bijaya bijaya 2.4M Nov 25 17:03 bin.18.fa  
-rw-rw-r-- 1 bijaya bijaya 1.7M Nov 25 17:03 bin.19.fa  
-rw-rw-r-- 1 bijaya bijaya 2.3M Nov 25 17:03 bin.1.fa  
-rw-rw-r-- 1 bijaya bijaya 988K Nov 25 17:03 bin.20.fa  
-rw-rw-r-- 1 bijaya bijaya 882K Nov 25 17:03 bin.21.fa  
-rw-rw-r-- 1 bijaya bijaya 1.6M Nov 25 17:03 bin.22.fa  
-rw-rw-r-- 1 bijaya bijaya 3.3M Nov 25 17:03 bin.23.fa  
-rw-rw-r-- 1 bijaya bijaya 501K Nov 25 17:03 bin.24.fa  
-rw-rw-r-- 1 bijaya bijaya 730K Nov 25 17:03 bin.25.fa  
-rw-rw-r-- 1 bijaya bijaya 4.3M Nov 25 17:03 bin.26.fa  
-rw-rw-r-- 1 bijaya bijaya 4.7M Nov 25 17:03 bin.27.fa  
-rw-rw-r-- 1 bijaya bijaya 821K Nov 25 17:03 bin.28.fa  
-rw-rw-r-- 1 bijaya bijaya 536K Nov 25 17:03 bin.29.fa  
-rw-rw-r-- 1 bijaya bijaya 1.8M Nov 25 17:03 bin.2.fa  
-rw-rw-r-- 1 bijaya bijaya 3.8M Nov 25 17:03 bin.30.fa  
-rw-rw-r-- 1 bijaya bijaya 542K Nov 25 17:03 bin.31.fa  
-rw-rw-r-- 1 bijaya bijaya 1.9M Nov 25 17:03 bin.3.fa  
-rw-rw-r-- 1 bijaya bijaya 418K Nov 25 17:03 bin.4.fa  
-rw-rw-r-- 1 bijaya bijaya 1.2M Nov 25 17:03 bin.5.fa  
-rw-rw-r-- 1 bijaya bijaya 581K Nov 25 17:03 bin.6.fa  
-rw-rw-r-- 1 bijaya bijaya 3.3M Nov 25 17:03 bin.7.fa  
-rw-rw-r-- 1 bijaya bijaya 3.4M Nov 25 17:03 bin.8.fa  
-rw-rw-r-- 1 bijaya bijaya 2.8M Nov 25 17:03 bin.9.fa  

# Creación de entorno para instalación de Quast y Busco

QUAST ya no se puede instalar con conda en Linux debido a conflictos irreparables de dependencias:

conda install -c bioconda quast

El paquete simplejson en conda-forge/bioconda NO tiene builds compatibles con Python 3.10+, y Quast lo requiere. Entonces, instalarlo con pip.

# conda create -n quast-env python=3.10  
# conda activate quast-env  
# pip install quast  

which quast.py  
which quast  

Creación del comando **quast** dentro del entorno. Esto crea un pequeño script que llama a quast.py con Python:  

echo -e '#!/bin/bash\npython /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast.py "$@"' > /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast  
chmod +x /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast  
quast --version      (QUAST v5.2.0)  

En Dataset

# quast.py megahit_output/final.contigs.fa -o quast_output

El directorio quast_output guarda los resultados de la evaluación y genera un informe con métricas de calidad del ensamblaje.  
total 724K  
drwxrwxr-x 2 bijaya bijaya 4.0K Dec  2 12:01 basic_stats  
-rw-rw-r-- 1 bijaya bijaya  53K Dec  2 12:01 icarus.html  
drwxrwxr-x 2 bijaya bijaya 4.0K Dec  2 12:01 icarus_viewers  
-rw-rw-r-- 1 bijaya bijaya 2.2K Dec  2 12:01 quast.log  
-rw-rw-r-- 1 bijaya bijaya 631K Dec  2 12:01 report.html      **  
-rw-rw-r-- 1 bijaya bijaya 1.3K Dec  2 12:01 report.tex  
-rw-rw-r-- 1 bijaya bijaya  552 Dec  2 12:01 report.tsv  
-rw-rw-r-- 1 bijaya bijaya 1.1K Dec  2 12:01 report.txt  
-rw-rw-r-- 1 bijaya bijaya 1.1K Dec  2 12:01 transposed_report.tex  
-rw-rw-r-- 1 bijaya bijaya  552 Dec  2 12:01 transposed_report.tsv  
-rw-rw-r-- 1 bijaya bijaya 1005 Dec  2 12:01 transposed_report.txt  


conda install -c bioconda busco

En Dataset

busco -i bins/bin_1.fa -o busco_output -l bacteria_odb10 -m geno



