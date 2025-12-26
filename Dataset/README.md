Los archivos con los que se están probando los scripts son:

- CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz   2.2G
- CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz   2.3G

Se pueden descargar de esta web: https://zenodo.org/records/5155395

```mermaid
flowchart LR


%% INPUT
A[FASTQ metagenómicos<br/>Datos crudos]:::input


%% QC
B[FastQC<br/>Control de calidad]:::tool
C[Fastp / Cutadapt<br/>Trimming lecturas]:::tool
D[Bowtie2 / HRRT<br/>Remoción ADN humano]:::tool
E[Lecturas microbioma<br/>Datos limpios]:::output


A --> B --> C --> D --> E


%% HUMANN CORE
F[MetaPhlAn2<br/>Preclasificación especies]:::tool
G[DIAMOND / Bowtie2<br/>Mapeo funcional]:::tool
H[UniRef90/50<br/>Familias génicas]:::database
I[MetaCyc<br/>Rutas metabólicas]:::database


J[Genes y rutas<br/>Abundancias normalizadas]:::output


E --> F --> G
H --> G --> J
J --> I --> J


%% VIRULENCE / ARG
K[DIAMOND BLASTx<br/>Genes patogénicos]:::tool
L[VFDB / CARD / ResFinder<br/>Virulencia y ARGs]:::database
M[Tablas VF–ARGs<br/>Abundancias]:::output


E --> K
L --> K --> M


%% TAXONOMY & DIVERSITY
N[MetaPhlAn4<br/>Perfil taxonómico]:::tool
O[Diversidad (R vegan)<br/>Alfa y beta]:::tool
P[NMDS<br/>Visualización]:::output


E --> N --> O --> P


%% STAT & BIOMARKERS
Q[DESeq2<br/>Análisis diferencial]:::tool
R[SPIEC-EASI<br/>Redes microbianas]:::tool
S[Random Forest<br/>Clasificación NAC]:::tool
T[Regresión logística<br/>Asociación clínica]:::tool


U[Biomarcadores<br/>Resultados finales]:::output


J --> Q --> U
J --> R --> U
J --> S --> U
U --> T --> U


%% CLASSES
classDef tool fill:#4F81BD,color:#ffffff,stroke:#2F5597
classDef database fill:#6AA84F,color:#ffffff,stroke:#38761D
classDef input fill:#B7B7B7,color:#000000,stroke:#7F7F7F
classDef output fill:#F6B26B,color:#000000,stroke:#E69138
```

