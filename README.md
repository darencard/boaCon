## Introduction

Welcome to the data repository for the *Boa constrictor* genome. See below for descriptions how to retrieve appropriate files in standard formats from public repositories.

## Publication

The raw genomic data and initial genome assembly were published in the following paper. Please cite this publication when using these data.

Bradnam KR, Fass JN, Alexandrov A, Baranay P, Bechner M, Birol I, Boisvert S, Chapman JA, Chapuis G, Chikhi R, Chitsaz H, Chou W, Corbeil J, Del Fabbro C, Docking TRR, Durbin R, Earl D, Emrich S, Fedotov P, Fonseca NA, Ganapathy G, Gibbs RA, Gnerre S, Godzaridis E, Goldstein S, Haimel M, Hall G, Haussler D, Hiatt JB, Ho I, Howard JT, Hunt M, Jackman SD, Jaffe DB, Jarvis ED, Jiang H, Kazakov S, Kersey PJ, Kitzman JO, Knight JR, Koren S, Lam T, Lavenier D, Laviolette F, Li Y, Li Z, Liu B, Liu Y, Luo R, MacCallum I, MacManes MD, Maillet N, Melnikov S, Naquin D, Ning Z, Otto TD, Paten B, Paulo OS, Phillippy AM, Pina-Martins F, Place M, Przybylski D, Qin X, Qu C, Ribeiro FJ, Richards S, Rokhsar DS, Ruby JG, Scalabrin S, Schatz MC, Schwartz DC, Sergushichev A, Sharpe T, Shaw TI, Shendure J, Shi Y, Simpson JT, Song H, Tsarev F, Vezzi F, Vicedomini R, Vieira BM, Wang J, Worley KC, Yin S, Yiu S, Yuan J, Zhang G, Zhang H, Zhou S, Korf IF. 2013. Assemblathon 2: evaluating de novo methods of genome assembly in three vertebrate species. *GigaScience 2(1)*. doi: [10.1186/2047-217x-2-10](https://doi.org/10.1186/2047-217x-2-10)

The raw transcriptomic data used for genome annotation and all genome annotations (i.e., repeats and genes) were published in the following paper. Please cite this publication when using these data.

Card DC, Adams RH, Schield DR, Perry BW, Corbin AB, Pasquesi GIM, Row K, Daza JM, Booth W, Montgomery CE, Boback SM, Castoe TA. In Review. Genomic basis of convergent island phenotypes in boa constrictors.

## Raw Genome Sequencing Data

The raw genome sequence read data for this genome can be obtained from the NCBI Sequence Read Archive under accession [ERP002294](https://trace.ncbi.nlm.nih.gov/Traces/sra/?study=ERP002294). Please cite Bradnam *et al*. (2013) when using these data.

## Raw Transcriptome Sequencing Data

The raw transcriptome sequence read data used for genome annotation can be obtained from the NCBI Sequence Read Archive under accession [SRP148755](https://trace.ncbi.nlm.nih.gov/Traces/sra/?study=SRP148755). Please cite Card *et al*. (In Review) when using these data.

Blood RNA-seq data used for annotation purposes was also obtained from a previously published dataset by [Vicoso *et al*. (2013)](https://doi.org/10.1371/journal.pbio.1001643). If using these data or all RNA-seq data, please be sure to cite this paper as well.

## Genome Assemblies

The raw genome assembly (FASTA format) used for annotation purposes can be found at GigaDB under [accession doi 10.5524/100060](http://dx.doi.org/10.5524/100060). Please cite Bradnam *et al*. (2013) when using these data.

The specific assembly is the snake 7C assembly produced by the SGA team. Here are direct links to the [scaffold assembly](ftp://parrot.genomics.cn/gigadb/pub/10.5524/100001_101000/100060/snake_7C_scaffolds.fa.gz) and [contig assembly](ftp://parrot.genomics.cn/gigadb/pub/10.5524/100001_101000/100060/snake_7C_contigs.fa.gz) (Note: Clicking links will download large files!).

Note that other assemblies are also available and were produced as part of the Assemblathon2 project, but the snake 7C-SGA team assembly was rated the best as part of this project and was used for annotation purposes. See Bradnam *et al*. (2013) for more information.

## Genome Annotation

Repeat and gene annotation files for the snake 7C-SGA team genome assembly (GFF and FASTA files), which are described in Card *et al*. (In Review), can be found at Figshare under [accession doi 10.6084/m9.figshare.9793013](https://doi.org/10.6084/m9.figshare.9793013). 

We have also created a [Genome Hub](https://genome.ucsc.edu/goldenpath/help/hgTrackHubHelp.html) that can be accessed through the [UCSC Genome Browser](https://genome.ucsc.edu/cgi-bin/hgHubConnect), followed by clicking on the “My Hubs” tab and then copying the URL [https://de.cyverse.org/anon-files/iplant/home/darencard/boaCon1_hub/assembly_hub.hub.txt](https://de.cyverse.org/anon-files/iplant/home/darencard/boaCon1_hub/assembly_hub.hub.txt) and clicking “Add Hub”. 

This resource contains repeat tracks and gene annotation tracks (`boaCon1_genes_raw`). There are additional gene annotation tracks for each species or database used to identify genes by homology, which provide more context about the putative identity of gene models.
