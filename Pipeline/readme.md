
Pipeline en Nextflow

main.nf

``` bash
nextflow.enable.dsl=2

params {
    reads1 = "Dataset/CAMISIM_Illumina_R1.anonymous.G0_T0.fq.gz"
    reads2 = "Dataset/CAMISIM_Illumina_R2.anonymous.G0_T0.fq.gz"
    outdir = "results"
    busco_lineage = "bacteria_odb10"
    busco_cpu = 16
}

/**********************************
 * FASTP
 **********************************/
process FASTP {

    tag "fastp"

    publishDir "${params.outdir}/fastp", mode: 'copy'

    conda "bioconda::fastp"

    input:
    path reads1
    path reads2

    output:
    path "R1_trimmed.fq.gz"
    path "R2_trimmed.fq.gz"
    path "fastp_report.html"
    path "fastp.json"

    script:
    """
    fastp \
      -i ${reads1} \
      -I ${reads2} \
      -o R1_trimmed.fq.gz \
      -O R2_trimmed.fq.gz \
      --html fastp_report.html \
      --json fastp.json
    """
}

/**********************************
 * MEGAHIT
 **********************************/
process MEGAHIT {

    tag "megahit"

    publishDir "${params.outdir}/megahit", mode: 'copy'

    conda "bioconda::megahit"

    input:
    path r1
    path r2

    output:
    path "megahit_output/final.contigs.fa"

    script:
    """
    megahit \
      -1 ${r1} \
      -2 ${r2} \
      -o megahit_output
    """
}

/**********************************
 * METABAT2
 **********************************/
process METABAT2 {

    tag "metabat2"

    publishDir "${params.outdir}/metabat2", mode: 'copy'

    conda "bioconda::metabat2"

    input:
    path contigs

    output:
    path "bins/*.fa"

    script:
    """
    mkdir -p bins
    metabat2 \
      -i ${contigs} \
      -o bins/bin
    """
}

/**********************************
 * QUAST
 **********************************/
process QUAST {

    tag "quast"

    publishDir "${params.outdir}/quast", mode: 'copy'

    conda "/home/warehouse/users/bijaya/Software/yes/envs/quast-env"

    input:
    path contigs

    output:
    path "quast_output"

    script:
    """
    quast.py ${contigs} -o quast_output
    """
}

/**********************************
 * BUSCO (por bin)
 **********************************/
process BUSCO {

    tag { bin.baseName }

    publishDir "${params.outdir}/busco", mode: 'copy'

    conda "busco-env"

    cpus params.busco_cpu

    input:
    path bin

    output:
    path "busco_${bin.baseName}"

    script:
    """
    busco \
      -i ${bin} \
      -o busco_${bin.baseName} \
      -l ${params.busco_lineage} \
      -m genome \
      --cpu ${task.cpus}
    """
}

/**********************************
 * WORKFLOW
 **********************************/
workflow {

    reads_ch = Channel.fromPath(params.reads1)
        .combine(Channel.fromPath(params.reads2))

    fastp_out = FASTP(reads_ch)

    contigs = MEGAHIT(
        fastp_out.out[0],
        fastp_out.out[1]
    )

    bins = METABAT2(contigs)

    QUAST(contigs)

    BUSCO(bins)
}
```



