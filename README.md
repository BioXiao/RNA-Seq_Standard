# RNA-Seq Standard

### Author: Yixiao Gong

RNA-Seq Standard Pipeline is designed to perform standard RNA-Seq analysis for Illumina TruSeq sequencing data. It generates a standard RNA-Seq HTML report which includes alignment quality assessment, sample distance evaluation, PCA plots, dispersion plots, MA plots, raw count/normalized count/fpkm table, pairwise differential expression analysis, top differential expressed genes heatmap, user-defined targets differential expression analysis/heatmap. 

#### Setting up:

1. You first need to have pre-built STAR reference in your [referenceFiles](referenceFiles/) folder. If you do not have it, please download a "genome.fa" file from public sites.
   Please use the following command to generate STAR Index:    
   
	```
	code/generate_STAR-Index params referenceFiles/STAR_Reference
	```
	
2. You need to have a "chromInfo.txt" file in your [referenceFiles](referenceFiles/) folder in order to produce RNA-Seq signal tracks. 

3. This pipeline can process GTC generated RNA-Seq data in the "/ifs/data/sequence/results/" folder. Please see the [template](meta_data/20160224.txt). Sample name need to be defined in this file for each of your samples. You will need to generate a .txt file for each of your fastq files directories put them into [meta data](meta_data/) directory and put path of those files into [params](params) file.

4. This pipeline can also perform automatic download from SRA and process the samples. Please see the [template](meta_data/sra_info.txt). Sample names need to be defined in this file for corresponding SRX numberi. The path of [sra_info.txt](meta_data/sra_info.txt) need to be put into [params](params) file. 

5. In [group information file](meta_data/group_info.txt) file, you need to categorize your sample into different groups to perform differential expression analysis. You can have multiple [group information file](meta_data/group_info.txt) with different names.

6. This pipeline use STAR aligner to align the reads. An transcripts annotation GTF format file ("--sjdbGTFfile" option for STAR aligner in the [params](params) file) is required for STAR to extract splice junctions and use them to greatly improve the accuracy of the mapping. And this step also counting number of reads per gene for all the genes in the transcripts annotation GTF file using in the alignment step. The downstream RNA-Seq standard report are all based on this transcript annotation GTF. 

7. If you have a signature gene list (for example you are doing knockdown of SPOP, then the list should include SPOP and some other related genes) which you would like to check the expression vaule immediately you can define it in the [signature gene list](meta_data/signature.txt). If you have a list of defined targets (for example some ChIP-Seq experiment define a list of direct targets of your knockdown gene), you can define them in the [target gene list](meta_data/target.txt). Or you can keep the existing link to the [signature gene list](meta_data/signature.txt). Please notice the name of the gene in these two file must be consistant with the names in the GTF file you used as "--sjdbGTFfile" option when you are doing STAR alignment. 


#### How to run it:

Once everything has been set up, you can run the pipeline as following:

1. Submit your jobs for each of your sample using the following command if you are using SGE:
   
	```
	qsub -b Y -cwd -pe threaded 1 ./run.bash params meta_data/group_info.txt
	```
	* If you are using another job scheduler, you can submit the follwoing command using your own job scheduler:
   
	```
	./run.bash params meta_data/group_info.txt
	```
	
2. If you have multiple group_info.txt file, you need to run the command for all your group_info.txt files.  

3. The summary html report will be generated in [pipeline/summarize](pipeline/summarize) folder.

#### Example of the html report:
[hESC Report By CellType](http://www.hpc.med.nyu.edu/~gongy05/RNA-Seq_Standard/H1_Cells/By_CellType/By_CellType.html)

[hESC Report By Tissue](http://www.hpc.med.nyu.edu/~gongy05/RNA-Seq_Standard/H1_Cells/By_Tissue/By_Tissue.html)
