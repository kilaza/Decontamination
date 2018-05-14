# Decontamination: Work in progress
This script show how to decontaminate biological sequences from the contaminant OTUs sequences. 
Sample collection, DNA isolation and purification are associated with possible introduction of contaminant DNA into the biological samples. Some possible contaminant includes sampling containers /bottles, sampling reagents, DNA isolation reagents, Scientist/technician and laboratory environments. Contaminants has significant effect especially when working with samples with low biomass composityion, which is the case in most clinical samples. Therefore, it is important to remove contaminant sequences / DNA so as to retain a true biological representation of the sampled site.


Important steps for the decontamination:

# Wetlab
#Prepare atleast three replicates of a blank controls spiked with known bacteria (sampling media or reagents can be used)
#Run the DNA isolation, library preparation and sequencing, similar to what is done in biological samples

# Drylab
#Run QC and OTU picking of controls and biological samples
#Run taxonomic annotation of Controls to identify sequences against their taxon
#Make comparison of the replicates by calculating percentage of reads for each plate
#If reads are comparable between replicates (compare percentages), then calculate average reads of each OTU detected
#Remove spiked taxa sequence/s to retain contaminant/background sequences from the controls
#Search for contamtaminant sequences in the biological sample sequences by aligning contaminant sequence against pre aligned biological sample sequences  at 100%, full length, using pynast algorithmn method and Uclast algorithmn for pairewise alignment
#If contaminants sequences are present in the biological sample, then contaminant sequences will map to specific sequence in biological sample sequences
#Then subtract average reads of contaminant sequence (OTU) from the mapped biological sample sequence (OTU). i.e subtraction of contaminant sequence reads/ OTUS will be done from the biological sample otu_table.
#After decontamination, taxonomic annotation of the biological sample sequences will be done (i.e decontaminated otu-table used).

# setup
#PBS -N qiime_20171016_NPall
#PBS -S /bin/bash
#PBS -q UCTlong 
#PBS -l nodes=1:series600:ppn=1
#PBS -V
#PBS -M kilaza.mwaikono@uct.ac.za
#PBS -d /researchdata/fhgfs/kmwaikono/project15_01/cbio-pipeline/uparse_downstream2/decont # point to the file where the iput files are located

source /home/kmwaikono/activate_qiime.sh # activate the qiime

inDir=/  # give the path to where your input files are located

outDir=/ # give the path to where your output files should be directed

align_seqs.py -i $inDir/conta.fa -o $outDir/decon100 -t $inDir/otus_repsetOUT.fa -e 250 -m PyNAST -a uclust -p 100.0

#conta.fa: Is a fasta file with sequences from the background of the spiked controls after removing known spiked bacteria
#decon100: Is a  folder where the output after aligningment will be directed (any name can be given)
#otus_repsetOUT.fa: Is a pree aligned fasta file with all biological sample sequences (from the nextflow output file /otus.align) 
#-e = 250 :Align sequences their entire length i.e 250bp
#-p = 100 :Percent sequence similarity between contaminant OTUs and Biological sample sequences
#-m = PyNAST : method for aligning sequences
#-a = uclust : method for performing pairwise alignmnent of sequences


# Output files
#conta_aligned.fasta: This a fasta file with sequences which aligned to biological sequences
#conta_failures.fasta: This is fasta file with sequences which did not align to biological sequences
#conta_log.txt: This is a summary file showing contaminant OTU which aligned to a given biological sample sequences. This
#summary will be used to locate contaminant OTUs of which their respective average sequence reads will be subtracted
#from matched biological OTUs. NB: conta - stands for contaminant,the user can give a different name.

# Subtracting contaminants reads from biological (target) samples
For all matched contaminant OTUs,  their average sequence reads will be subtracted from the respective OTUs in biological sample otu-table.txt (the otu-table.txt is the nextflow output in the folder otu-picking/)

# Decontaminated otu-table
If the number of sequence reads in the contaminant  OTU is higher than in their respective OTU in the biological sample, then the entire OTU will be removed from the biological sample otu-table, otherwise, the equivalent average reads will be substracted. The decontaminated OTU table can then be used for the taxonomic annotation.

