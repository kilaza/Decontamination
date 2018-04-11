# Decontamination: Work in progress
This script show how to decontaminate biological sequences from the contaminant OTUs sequences. 
Sample collection, DNA isolation and purification are associated with possible introduction of contaminant DNA into the biological samples. Some possible contaminant includes sampling containers /bottles, sampling reagents, DNA isolation reagents, Scientist/technician and laboratory environments. Contaminants has significant effect especially when working with samples with low biomass composityion, which is the case in most clinical samples. Therefore, it is important to remove contaminant sequences / DNA so as to retain a true biological representation of the sampled site.


Important steps for the decontamination:

# Wetlab
#Prepare atleast three replicates of a blank controls spiked with known bacteria (sampling media or reagents can be used)
#Run the DNA isolation, library preparation and sequencing, similar to what is done in biological samples

# Drylab
#Run Qc and OTU picking of controls and biological samples
#Run taxonomic annotation of Controls to identify sequences against their taxon
#make comparison of the replicates by calculating percentage of reads for each detected reads
#If reads are comparable between replicates ( compare percentages), then calculate average reads of each OTU detected
#Remove spiked taxa sequence/s to retain contaminant/background sequences from the controls
#Search for contamtaminat sequences in the biological sample sequences by aligning contaminant sequence against biological sample sequences  at 100%, full length, using pynast algorithmn
#if contaminants are present in the biological sample, then contaminant sequences will map to specific sequence in biological sample
#decontamination will be done by subtracting average reads of contaminant sequence (OTU) from the mapped biological sample sequence (OTU). i.e subtraction of contaminant sequence reads/ OTUS will be done from the biological sample otu_table.
#After decontamination, taxonomic annotation of the biological sample sequences will be done (i.e decontaminated otu-table used).

# setup
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

#conta.fa: Is a fasta file with sequences from spiked controls after removing known spiked bacteria
#decon100: Is a  folder where the output after aligningment will be directed
#otus_repsetOUT.fa: Is a fasta file with all biological sequences
#-e = 250  - align sequences for their entire length i.e 250bp
#-p = 100, percent sequence similarity is set to 100%.
#PyNAST is a default alignment method.

# Output files
#conta_aligned.fasta: This a fasta file with sequences which aligned to biological sequences
#conta_failures.fasta: This is fasta file with sequences which did not align to any biological sequences
#conta_log.txt: This is a summary file showing which contaminant OTU aligned to a given biological sample. this
#summary will be used to locate contaminant OTUs of which their respective average sequence reads will be subtracted
#from matched biological OTUs. NB: conta - stands for contaminant,the user can assign anyother name.

# Subtracting contaminants reads from biological (target) samples.. in progress...

