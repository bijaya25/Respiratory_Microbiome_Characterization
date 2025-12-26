Los archivos con los que se están probando los scripts son:

- CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz   2.2G
- CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz   2.3G

Se pueden descargar de esta web: https://zenodo.org/records/5155395

```mermaid
flowchart LR

%% INPUT
A[FASTQ metagenomicos<br/>Datos crudos]

%% QC
B[FastQC<br/>Control de calidad]
C[Fastp o Cutadapt<br/>Trimming lecturas]
D[Bowtie2 o HRRT<br/>Remocion ADN humano]
E[Lecturas microbioma<br/>Datos limpios]

A --> B --> C --> D --> E

%% HUMANN CORE
F[MetaPhlAn2<br/>Preclasificacion especies]
G[DIAMOND o Bowtie2<br/>Mapeo funcional]
H[UniRef90 o UniRef50<br/>Familias genicas]
I[MetaCyc<br/>Rutas metabolicas]

J[Genes y rutas<br/>Abundancias normalizadas]

E --> F --> G
H --> G --> J
J --> I --> J

%% VIRULENCE y ARG
K[DIAMOND BLASTx<br/>Genes patogenicos]
L[VFDB CARD ResFinder<br/>Virulencia y ARGs]
M[Tablas VF y ARGs<br/>Abundancias]

E --> K
L --> K --> M

%% TAXONOMIA y DIVERSIDAD
N[MetaPhlAn4<br/>Perfil taxonomico]
O[Diversidad R vegan<br/>Alfa y beta]
P[NMDS<br/>Visualizacion]

E --> N --> O --> P

%% ESTADISTICA y BIOMARCADORES
Q[DESeq2<br/>Analisis diferencial]
R[SPIEC EASI<br/>Redes microbianas]
S[Random Forest<br/>Clasificacion NAC]
T[Regresion logistica<br/>Asociacion clinica]

U[Biomarcadores<br/>Resultados finales]

J --> Q --> U
J --> R --> U
J --> S --> U
U --> T --> U

```

