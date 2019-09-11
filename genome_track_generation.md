### Commands used to generate the genome tracks

File key

```
boaCon.assembly.fasta = scaffold genome assembly fasta
boaCon.maker_genes_only.gff3 = GFF3 file produced from MAKER annotation with only gene annotations included (no sequences)
boaCon.repeats.out = RepeatMasker .out file
```

Commands to prepare assembly

```
faToTwoBit boaCon.assembly.fasta boaCon.assembly.2bit
cat boaCon.assembly.fasta | bioawk -c fastx '{ print $name, length($seq) }' > boaCon.assembly.chrom.sizes
```

Commands to prepare repeat annotation bigBed file.

```
cat boaCon.repeats.out | tail -n +4 | awk -v OFS="\t" '{ print $5, $6+1, $7, $11, $1, $9 }' | awk -v OFS="\t" '{ if ($5 > 1000) print $1, $2, $3, $4, "1000", $6; else print $0 }' | awk -v OFS="\t" '{ if ($6 == "C") print $1, $2, $3, $4, $5, "-"; else print $0 }' | sort -k1,1 -k2,2n > boaCon.repeats.bed6
bedToBigBed boaCon.repeats.bed6 boaCon.assembly.chrom.sizes boaCon.repeats.bigBed
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
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS25_Boa_Python_GCF_000186305.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ if (replace != "") print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12; else print $1, $2, $3, $4"|No_homology_match", $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Python.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS26_Boa_Thamnophis_GCF_001077635.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ if (replace != "") print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12; else print $1, $2, $3, $4"|No_homology_match", $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Thamnophis.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS27_Boa_Anolis_GCF_000090745.1_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ if (replace != "") print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12; else print $1, $2, $3, $4"|No_homology_match", $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Anolis.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS28_Boa_Human_GCF_000001405.37_homology_annotation.fmt6.txt | awk '{ print $4 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ if (replace != "") print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12; else print $1, $2, $3, $4"|No_homology_match", $5, $6, $7, $8, $9, $10, $11, $12 }'; done > boaCon.maker_genes_only.sorted.Human.bed12
```

Commands to create bed12 files for the gene annotation track with homology features to InterPro and SwissProt, which can be converted to bigBed as above.

```
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; replace=`grep -w "${query}" FileS23_BoaCon_SwissProt_blastp_1e-5.fmt6.txt | awk '{ print $2 }' | paste -s -d ";" -`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ if (replace != "") print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12; else print $1, $2, $3, $4"|No_homology_match", $5, $6, $7, $8, $9, $10, $11, $12 }'; done | sort -k1,1 -k2,2n > boaCon.maker_genes_only.sorted.SwissProt.bed12
cat boaCon.maker_genes_only.sorted.bed12 | while read line; do query=`echo ${line} | awk '{ print $4 }'`; grep -w "${query}" FileS24_BoaCon_interproscan_full.tsv | grep -v "MobiDBLite" | while read hit; do replace=`echo ${hit} | awk '{ print $4":"$5 }'`; echo ${line} | awk -v OFS="\t" -v replace="${replace}" '{ print $1, $2, $3, $4"|"replace, $5, $6, $7, $8, $9, $10, $11, $12 }'; done; done | uniq | sort | uniq > boaCon.maker_genes_only.IntroPro.bed12
```

Script to format the assembly hub
```python
import trackhub
import re
import sys
import os
import glob

# In contrast to the example in the README, we do not use the
# `trackhub.default_hub` function but instead build up the hub from its
# component pieces.
hub = trackhub.Hub(
    "assembly_hub",
    short_label="boaCon1 hub",
    long_label="Assembly hub for Boa constrictor",
    email="daren.card@gmail.com")

# The major difference from a regular track hub is this object, which needs
# to be added to the genomes_file object:
genome = trackhub.Assembly(
    genome="boaCon1",
    twobit_file="boaCon1.2bit",
    organism="boa constrictor",
    defaultPos="scaffold-3:0-1000000",
    scientificName="Boa constrictor",
    description="Boa constrictor V1",
    html_string="Boa constrictor V1 INFO\n",
    orderKey=4800
)

genomes_file = trackhub.GenomesFile()
hub.add_genomes_file(genomes_file)

# we also need to create a trackDb and add it to the genome
trackdb = trackhub.TrackDb()
genome.add_trackdb(trackdb)

# add the genome to the genomes file here:
genomes_file.add_genome(genome)

# Find all bigBeds
# must only be a single "."
for bb in glob.glob("boaCon1*.bigBed"):
    os.path.basename(bb).split(".")
    name, _ = os.path.basename(bb).split(".")
    track = trackhub.Track(
        name=trackhub.helpers.sanitize(name),
        source=bb,
        tracktype='bigBed')
    trackdb.add_tracks(track)

# Assembly hubs also need to have a Group specified. Here's how to do that:
main_group = trackhub.groups.GroupDefinition(
    "boaCon1_tracks",
    label="boaCon1 Tracks",
    priority=1,
    default_is_closed=False)

groups_file = trackhub.groups.GroupsFile([main_group])
genome.add_groups(groups_file)

# We can now add the "group" parameter to all the children of the trackDb
for track in trackdb.children:
    track.add_params(group="boaCon1_tracks")

# render/stage the hub for upload
trackhub.upload.stage_hub(hub, staging="boaCon1-staging")
```

These represent some tailored commands for this genome, but otherwise the steps outlined in [my blog post](http://darencard.net/blog/2019-01-25-UCSC-genome-track-setup/) were followed.
