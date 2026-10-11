# Colorectal cancer RNA-seq: from raw reads to pathways

Paired RNA-seq analysis of colorectal cancer (GSE50760), done from raw FASTQ files to pathways: QC, Salmon quantification, DESeq2 with a paired design, and GO/GSEA.

[Open in Colab](https://colab.research.google.com/github/rauanu-cas9/Colorectal-cancer-RNA-seq-/blob/main/crc_rnaseq_from_fastq.ipynb)

# Question

What distinguishes colorectal tumour from normal colon tissue in the same patient, and what distinguishes a liver metastasis from the primary tumour?

# Data

[GSE50760](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE50760) (Kim et al., 2015): normal colon, primary tumour and liver metastasis from the same patients. This run uses 6 of the 18 patients (18 samples) and the first 2M read pairs per sample, so it fits in a free Colab session (about an hour)

# Pipeline

1. Sample sheet from ENA and GEO
2. Trimming and QC: fastp, FastQC, MultiQC
3. Quantification against GENCODE v46: Salmon
4. Differential expression with the design patient + tissue`: DESeq2 (pydeseq2)
5. PCA, volcano plots, heatmap, check against known CRC markers
6. Pathways: Enrichr (GO Biological Process) and GSEA (MSigDB Hallmark)

# Main results

90-92% of reads mapped to the transcriptome in every sample.
Tissue separates samples on PC1 and PC2, patient drives PC3 to PC5, which is why patient is in the model.
Tumour vs normal: 836 differentially expressed genes (402 up, 434 down). All 10 reference CRC markers move in the expected direction; 6 of 10 are individually significant at padj < 0.05
Up in tumour: E2F and MYC targets, G2-M checkpoint. Down: inflammatory response, oxidative phosphorylation, fatty acid metabolism.
Metastasis vs primary: 215 genes, dominated by liver genes (FGA, FGB, ALB, HP and others). This is host liver tissue in the sample, not a property of the tumour cells.

# Files

crc_rnaseq_from_fastq.ipynb: the full analysis, FASTQ to figures
crc_rnaseq_report.pdf: two-page summary of the results
results: differential expression tables, normalised counts, sample sheet, MultiQC report

# Limitations

Only 6 of 18 patients and a 2M read-pair subsample per sample, so small effects and weakly expressed genes are underpowered. The Salmon index has no decoys, and gene counts are summed NumReads without tximport-style length correction. Tumour samples contain stroma and immune cells, and no deconvolution was done. The results show the workflow and its sanity checks, not new biology.

## License

MIT, see [LICENSE](LICENSE)
