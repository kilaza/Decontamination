# Decontamination
This script show how to decontaminate biological sequences from contaminant OTUs sequences
Since sample collection, DNA isolation and purification are associated with possible introduction of contaminant DNA apart from the DNA from intended samples. Some possible contaminant soiurces are sampling containers /vials, sampling reagents, DNA isolation reagents, Scientist/technician and lab environments. It is important to remove contaminant so as to retain a true biological representation of the sampled site. Indeed this is very critical in clinical samples where, sli


#!/bin/bash
#PBS -N qiime_20171016_NPall
#PBS -S /bin/bash
#PBS -q CLOUDHMQ # not necessarily to use this ...
#PBS -l nodes=1:series800:ppn=4 3 not necesarily to use this..
#PBS -V
#PBS -M kilaza.mwaikono@uct.ac.za
#PBS -d /researchdata/fhgfs/kmwaikono/project15_01/cbio-pipeline/uparse_downstream2/decont # point to the file where the iput files are located

source /home/kmwaikono/activate_qiime.sh # activate the qiime

inDir=/  # give the path to where your input files are located

outDir=/ # give the path to where your output should be directed


align_seqs.py -i $inDir/conta.fa -o $outDir/decon100 -t $inDir/otus_repsetOUT.fa -e 250 -p 100.0

#conta.fa is a fasta file with sequences from spiked controls after removing known spiked bacteria
#decon100 is a  folfer where the output after aligningment will be stored
#otus_repsetOUT.fa is a fasta file with all biological sequences
#-e = 250  - align sequences for their entire length i.e 250bp
#-p = 100, percent sequence similarity is set to 100%.
#PyNAST is a default alignment method.

## output will be three files
#conta_aligned.fasta - this a fasta file with sequences which aligned to biological sequences
#conta_failures.fasta - this is fasta file with sequences which did not align to any biological sequences
#conta_log.txt- this is a summary file showing which contaminant OTU aligned to a given biological sample. this
#summary will be used to locate contaminant OTUs of which their respective average sequence reads will be subtracted
#from matched biological OTUs. NB: conta - stands for contaminant,the user can assign anyother name.

##subtracting contaminants reads from biological (target) samples.. in progress...

