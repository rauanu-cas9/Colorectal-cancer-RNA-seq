# Colorectal cancer RNA-seq: from raw reads to pathways

I did this one from raw FASTQ files and not from a ready count table, because I wanted to go through the whole workflow once from start to end: QC, quantification, DESeq2 with a paired design, then GO and GSEA.

[Open in Colab](https://colab.research.google.com/github/rauanu-cas9/Colorectal-cancer-RNA-seq-/blob/main/crc_rnaseq_from_fastq.ipynb)

## Question

What is different between colorectal tumour and normal colon tissue from the same patient? And how does a liver metastasis differ from the primary tumour?

## Data

[GSE50760](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE50760) (Kim et al., 2015): normal colon, primary tumour and liver metastasis from the same patients. I used 6 of the 18 patients (18 samples) and only the first 2M read pairs of each sample, so it runs in a free Colab session in about an hour. I have not run all 18 patients.

## Pipeline

1. Sample sheet from ENA and GEO
2. Trimming and QC with fastp, FastQC and MultiQC
3. Quantification with Salmon against GENCODE v46
4. Differential expression with DESeq2 (pydeseq2), design `patient + tissue`
5. PCA, volcano plots, a heatmap, and a check against known CRC marker genes
6. Pathways with Enrichr (GO Biological Process) and GSEA (MSigDB Hallmark)

## What I found

- Mapping was fine: 90 to 92% of reads mapped to the transcriptome in every sample.
- Tissue separates the samples on PC1 and PC2. Patient drives PC3 to PC5, and that is why patient is in the model.
- Tumour vs normal: 836 genes differ (402 up, 434 down). All 10 CRC marker genes I checked move in the expected direction, and 6 of them are significant on their own (padj < 0.05).
- Up in tumour: E2F and MYC targets, G2-M checkpoint. Down: inflammatory response, oxidative phosphorylation, fatty acid metabolism.
- Metastasis vs primary: 215 genes, mostly liver genes (FGA, FGB, ALB, HP and others). Most likely this is host liver tissue in the sample and not something the tumour cells do.

## Limits

Only 6 of 18 patients and 2M read pairs per sample, so small effects and weakly expressed genes can be missed. The Salmon index has no decoys, and I summed NumReads per gene without a length correction like tximport does. Tumour samples contain stroma and immune cells, and I did not deconvolve them. So this shows the workflow and its sanity checks, not new biology.

## Files

- [crc_rnaseq_from_fastq.ipynb](crc_rnaseq_from_fastq.ipynb): the full analysis, FASTQ to figures
- [crc_rnaseq_report.pdf](crc_rnaseq_report.pdf): two page summary of the results

## License

MIT, see [LICENSE](LICENSE).
