### Commands used to generate the genome tracks

File key

```
boaCon.assembly.fasta = scaffold genome assembly fasta
boaCon.maker_genes_only.gff3 = GFF3 file produced from MAKER annotation with only gene annotations included (no sequences)
boaCon.repeats.gff3 = GFF3 file produced from MAKER annotation with only repeat annotations included (no sequences)
```

Commands to prepare assembly

```
faToTwoBit boaCon.assembly.fasta boaCon.assembly.2bit
cat boaCon.assembly.fasta | bioawk -c fastx '{ print $name, length($seq) }' > boaCon.assembly.chrom.sizes
```

Commands to prepare repeat annotation bigBed file.

```

```

Commands to prepare gene annotation bigBed file for base annotation.

```
gff3ToGenePred boaCon.maker_genes_only.gff3 boaCon.maker_genes_only.genePred
genePredToBed boaCon.maker_genes_only.genePred boaCon.maker_genes_only.bed12
sort-bed boaCon.maker_genes_only.bed12 > boaCon.maker_genes_only.sorted.bed12
bedToBigBed boaCon.maker_genes_only.sorted.bed12 boaCon.assembly.chrom.sizes boaCon.maker_genes_only.bigBed
```

Commands to create bed12 files for the gene annotation track with homology features inferred from 4 other vertebrate species, which can be converted to bigBed as above.

```
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS25_Boa_Python_GCF_000186305.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Python.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS26_Boa_Thamnophis_GCF_001077635.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Thamnophis.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS27_Boa_Anolis_GCF_000090745.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Anolis.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS28_Boa_Human_GCF_000001405.37_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Human.bed12
```

Commands to create bed12 files for the gene annotation track with homology features to InterPro and SwissProt, which can be converted to bigBed as above.

```
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS23_BoaCon_SwissProt_blastp_1e-5.fmt6.txt | awk '{ print $2 }' | paste -s -d ";" -`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.SwissProt.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS24_BoaCon_interproscan_full.tsv | grep -v "MobiDBLite" | awk -F "\t" '{ print $4":"$5 }' | sort -k1,1 | paste -s -d "|" -`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.IntroPro.bed12
```

These represent some tailored commands for this genome, but otherwise the steps outlined in [my blog post](http://darencard.net/blog/2019-01-25-UCSC-genome-track-setup/) were followed.
