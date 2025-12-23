# Pipeline bioinformático en Bash

## Instalación y ejecución de fastp

``` bash
conda install -c bioconda fastp
```
  
En carpeta Dataset dentro de la carpeta de proyecto microbioma:

``` bash
fastp -i CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz -I CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz -o R1_trimmed.fq.gz -O R2_trimmed.fq.gz --html fastp_report.html
```

Se generaron los archivos:  

``` bash
-rw-rw-r--   1 bijaya bijaya 2.2G Nov 25 16:05 R1_trimmed.fq.gz    **  
-rw-rw-r--   1 bijaya bijaya 2.2G Nov 25 16:05 R2_trimmed.fq.gz    **  
-rw-rw-r--   1 bijaya bijaya 135K Nov 25 16:05 fastp.json  
-rw-rw-r--   1 bijaya bijaya 477K Nov 25 16:05 fastp_report.html   **
```

## Instalación y ejecución de megahit

``` bash
conda install -c bioconda megahit
```

En Dataset

``` bash
megahit -1 R1_trimmed.fq.gz -2 R2_trimmed.fq.gz -o megahit_output
```

Se genera la carpeta megahit_output con los archivos:  

``` bash
-rw-rw-r-- 1 bijaya bijaya  262 Nov 25 16:54 checkpoints.txt  
-rw-rw-r-- 1 bijaya bijaya    0 Nov 25 16:54 done  
-rw-rw-r-- 1 bijaya bijaya 319M Nov 25 16:54 final.contigs.fa       **  
drwxrwxr-x 2 bijaya bijaya 4.0K Nov 25 16:54 intermediate_contigs  
-rw-rw-r-- 1 bijaya bijaya 161K Nov 25 16:54 log  
-rw-rw-r-- 1 bijaya bijaya  993 Nov 25 16:19 options.json  
```

## Instalación y ejecución de metabat2

``` bash
conda install -c bioconda metabat2
``` 

En Dataset

``` bash
metabat2 -i megahit_output/final.contigs.fa -o bins/bin
```

MetaBAT2 generará archivos de binning en el directorio bins/, donde cada archivo de bin representará un MAG:  

``` bash
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
```

## Creación de entorno para instalación de Quast

QUAST ya no se puede instalar con conda en Linux debido a conflictos irreparables de dependencias:

``` bash
conda install -c bioconda quast
```

El paquete simplejson en conda-forge/bioconda NO tiene builds compatibles con Python 3.10+, y Quast lo requiere. Entonces, instalarlo con pip.

``` bash
# conda create -n quast-env python=3.10  
# conda activate quast-env  
# pip install quast  
```

``` bash
which quast.py  
which quast
```

Creación del comando **quast** dentro del entorno. Esto crea un pequeño script que llama a quast.py con Python:  

``` bash
echo -e '#!/bin/bash\npython /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast.py "$@"' > /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast  
chmod +x /home/warehouse/users/bijaya/Software/yes/envs/quast-env/bin/quast  
quast --version      (QUAST v5.2.0)  
```

En Dataset

``` bash
quast.py megahit_output/final.contigs.fa -o quast_output
```

El directorio quast_output guarda los resultados de la evaluación y genera un informe con métricas de calidad del ensamblaje.  

``` bash
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
```

## Creación de entorno exclusivo para Busco

BUSCO sí funciona perfectamente con conda, pero requiere un entorno aislado y versiones estrictas, no basta con ejecutar:  
conda install -c bioconda busco

BUSCO requiere: Python 3.7–3.10 , augustus , blast o diamond , hmmer  

``` bash
conda create -n busco-env python=3.10 -y
conda activate busco-env 
conda install -c conda-forge -c bioconda busco=5.7.1
busco --version      (BUSCO 5.7.1)  
```

En Dataset

``` bash
busco -i bins/bin.1.fa -o test_bin1 -l bacteria_odb10 -m genome
```

-l bacteria_odb10 especifica el conjunto de referencia de genes (en este caso, para bacterias).
-m genome le indica a BUSCO que use un modo genómico.  

Resultado:  

    ---------------------------------------------------
    |Results from dataset bacteria_odb10               |
    ---------------------------------------------------
    |C:66.9%[S:66.9%,D:0.0%],F:8.1%,M:25.0%,n:124      |
    |83    Complete BUSCOs (C)                         |
    |83    Complete and single-copy BUSCOs (S)         |
    |0    Complete and duplicated BUSCOs (D)           |
    |10    Fragmented BUSCOs (F)                       |
    |31    Missing BUSCOs (M)                          |
    |124    Total BUSCO groups searched                |
    ---------------------------------------------------

## Ejecución de BUSCO en bucle:

``` bash
for file in bins/*.fa; do
    name=$(basename "$file" .fa)
    busco -i "$file" \
          -o "busco_${name}" \
          -l bacteria_odb10 \
          -m genome \
          --cpu 16 &
done
```

Se generaron las carpetas por cada bin:  

``` bash
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.1  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.10  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.11  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.12  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.13  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.14  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.15  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:26 busco_bin.16  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.17  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.18  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.19  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.2  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.20  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:26 busco_bin.21  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.22  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.23  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.24  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.25  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.26  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.27  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.28  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.29  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.3  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.30  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.31  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:26 busco_bin.4  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.5  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.6  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:25 busco_bin.7  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.8  
drwxrwxr-x 5 bijaya bijaya 4.0K Dec  2 13:27 busco_bin.9  
```

Observando rápidamente los resultados por cada bin:

find . -name "short_summary.txt" -exec awk '/C:/{print FILENAME "\t" $0}' {} \;  

``` bash
./busco_bin.28/run_bacteria_odb10/short_summary.txt             C:19.4%[S:19.4%,D:0.0%],F:10.5%,M:70.1%,n:124      
./busco_bin.26/run_bacteria_odb10/short_summary.txt             C:37.1%[S:33.1%,D:4.0%],F:12.9%,M:50.0%,n:124      
./busco_bin.11/run_bacteria_odb10/short_summary.txt             C:71.0%[S:62.9%,D:8.1%],F:5.6%,M:23.4%,n:124       
./busco_bin.7/run_bacteria_odb10/short_summary.txt              C:82.3%[S:69.4%,D:12.9%],F:8.1%,M:9.6%,n:124       
./busco_bin.14/run_bacteria_odb10/short_summary.txt             C:57.3%[S:56.5%,D:0.8%],F:14.5%,M:28.2%,n:124      
./busco_bin.10/run_bacteria_odb10/short_summary.txt             C:71.0%[S:70.2%,D:0.8%],F:3.2%,M:25.8%,n:124       
./busco_bin.17/run_bacteria_odb10/short_summary.txt             C:53.2%[S:52.4%,D:0.8%],F:14.5%,M:32.3%,n:124      
./busco_bin.20/run_bacteria_odb10/short_summary.txt             C:20.2%[S:20.2%,D:0.0%],F:8.9%,M:70.9%,n:124       
./busco_bin.24/run_bacteria_odb10/short_summary.txt             C:12.1%[S:12.1%,D:0.0%],F:7.3%,M:80.6%,n:124       
./busco_bin.6/run_bacteria_odb10/short_summary.txt              C:13.7%[S:13.7%,D:0.0%],F:4.8%,M:81.5%,n:124       
./busco_bin.25/run_bacteria_odb10/short_summary.txt             C:6.5%[S:6.5%,D:0.0%],F:4.8%,M:88.7%,n:124         
./busco_bin.23/run_bacteria_odb10/short_summary.txt             C:96.8%[S:96.0%,D:0.8%],F:0.8%,M:2.4%,n:124        
./busco_bin.9/run_bacteria_odb10/short_summary.txt              C:59.7%[S:39.5%,D:20.2%],F:3.2%,M:37.1%,n:124      
./busco_bin.22/run_bacteria_odb10/short_summary.txt             C:42.7%[S:41.9%,D:0.8%],F:16.9%,M:40.4%,n:124      
./busco_bin.30/run_bacteria_odb10/short_summary.txt             C:89.5%[S:86.3%,D:3.2%],F:1.6%,M:8.9%,n:124        
./busco_bin.3/run_bacteria_odb10/short_summary.txt              C:46.8%[S:46.8%,D:0.0%],F:8.9%,M:44.3%,n:124       
./busco_bin.5/run_bacteria_odb10/short_summary.txt              C:25.0%[S:25.0%,D:0.0%],F:10.5%,M:64.5%,n:124      
./busco_bin.18/run_bacteria_odb10/short_summary.txt             C:88.7%[S:87.1%,D:1.6%],F:5.6%,M:5.7%,n:124        
./busco_bin.15/run_bacteria_odb10/short_summary.txt             C:8.1%[S:8.1%,D:0.0%],F:4.8%,M:87.1%,n:124         
./busco_bin.27/run_bacteria_odb10/short_summary.txt             C:97.6%[S:97.6%,D:0.0%],F:0.8%,M:1.6%,n:124        
./busco_bin.29/run_bacteria_odb10/short_summary.txt             C:5.6%[S:5.6%,D:0.0%],F:5.6%,M:88.8%,n:124         
./busco_bin.19/run_bacteria_odb10/short_summary.txt             C:68.5%[S:68.5%,D:0.0%],F:8.9%,M:22.6%,n:124       
./busco_bin.31/run_bacteria_odb10/short_summary.txt             C:19.4%[S:19.4%,D:0.0%],F:8.1%,M:72.5%,n:124       
./test_bin1/run_bacteria_odb10/short_summary.txt                C:66.9%[S:66.9%,D:0.0%],F:8.1%,M:25.0%,n:124       
./busco_bin.4/run_bacteria_odb10/short_summary.txt              C:4.0%[S:4.0%,D:0.0%],F:4.0%,M:92.0%,n:124         
./busco_bin.21/run_bacteria_odb10/short_summary.txt             C:16.9%[S:16.1%,D:0.8%],F:8.9%,M:74.2%,n:124       
./busco_bin.13/run_bacteria_odb10/short_summary.txt             C:16.9%[S:16.9%,D:0.0%],F:12.9%,M:70.2%,n:124      
./busco_bin.1/run_bacteria_odb10/short_summary.txt              C:66.9%[S:66.9%,D:0.0%],F:8.1%,M:25.0%,n:124       
./busco_bin.8/run_bacteria_odb10/short_summary.txt              C:91.9%[S:91.1%,D:0.8%],F:2.4%,M:5.7%,n:124        
./busco_bin.12/run_bacteria_odb10/short_summary.txt             C:53.2%[S:53.2%,D:0.0%],F:1.6%,M:45.2%,n:124 
``` 
