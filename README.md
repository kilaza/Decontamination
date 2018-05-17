# Decontamination of Microbiome Sequence data
Decontamination is a process of removing contaminant sequence (OTUs) from the biological / taregt samples sequences data. Reported potential sources of contaminants are the DNA isolation and purification reagents, sample storage media, sampling tools, laboratory environments and researchers. This module points some of the aspects to implemented in the wetlab as well as in the dtrylab for the efficient implementation of the deconatmanation process. Lastly, provides a script which can detect and remove contaminant sequence from the target microbome sequence data.

# To be implemented in the Wetlab
Atleast three replicate of a blank (or sampling media) spiked with known pure bacteria isolate (prefereably not expected in your study profile) should be prepared. the spiking concetration should be comparable to the concentration in the target biological sample. If there is high disperity of the DNA concentration levels betweem biological samples, then, spiked controls can as wellbe prepared at both the higher levels and lower levels. The objective is to introduce a relatively similar competition of the DNA from different OTUs during the amplification process. Run the DNA isolation, library preparation and sequencing in exactly the same ways as in the tareget biological samples.

# To be implemented in the Bionformatic lab
1. Run QC and OTU picking and the taxonomic annotation of the spiked controls and biological samples separatedly
2. Check reproducibility of the controls replicates by comparing percentage of reads in each replicate
4. If reads are comparable between replicates, then calculate average reads of each OTU detected
5. Remove the spiked OTUs (the most abundant) retain only the contaminants / background sequences
6. Search for contaminant sequences in the biological sample sequences by aligning contaminant sequence against the pre-aligned biological sample sequences at 100%, full length (i.e 25bbp), using PyNAST method and Uclust algorithmn for pairewise alignment
7. If contaminants are present in the biological sample, then contaminant sequences will map to specific sequence in the biological sample sequences
-Then, the average reads of the contaminant OTUs sequences will be substracted from the mapped biological sample OTU sequences. i.e subtraction of contaminant sequence reads/ OTUS will be done from the biological sample otu_table.

## Required files and decontamination parameter setting

conta.fa: Is a fasta file with sequences from the background of the spiked controls after removing known spiked bacteria
decon100: Is a  folder where the output after aligningment will be directed (any name can be given)
otus_repsetOUT.fa: Is a pree aligned fasta file with all biological sample sequences (from the nextflow output file /otus.align) 
-e = 250 :Align sequences their entire length i.e 250bp
-p = 100 :Percent sequence similarity between contaminant OTUs and Biological sample sequences
-m = PyNAST : method for aligning sequences
-a = uclust : method for performing pairwise alignmnent of sequences
# Output files
conta_aligned.fasta: This a fasta file with sequences which aligned to biological sequences
conta_failures.fasta: This is fasta file with sequences which did not align to biological sequences
onta_log.txt: This is a summary file showing contaminant OTU which aligned to a given biological sample sequences. Thissummary will be used to locate contaminant OTUs of which their respective average sequence reads will be subtracted from matched biological OTUs. NB: conta - stands for contaminant,the user can give a different name.

# Subtracting contaminants reads from biological (target) samples
For all matched contaminant OTUs, their average sequence reads will be subtracted from the respective OTUs in biological sample otu-table.txt (the otu-table.txt is the nextflow output in the folder otu-picking/)

# Decontaminated otu-table
If the number of sequence reads in the contaminant  OTU is higher than in their respective OTU in the biological sample, then the entire OTU will be removed from the biological sample otu-table, otherwise, the equivalent average reads will be substracted. The decontaminated OTU table can then be used for the taxonomic annotation.

