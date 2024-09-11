OmiGA: a toolkit for Omics Genetic Analysis

📖 OmiGA v0.4.0-beta1 User Manual

🗓 Date of Modification: 15th August, 2024



⚠️ Confidentiality Notice: 

	‼️ This document and software is unpublished and confidential. Please do not distribute or forward this document to anyone outside of FarmGTEx without explicit permission; 

	‼️ OmiGA has primarily been tested on Linux systems, and it has not yet been tested on other platforms such as Windows and Mac.

🌿Major updates (OmiGA v0.4.0-beta1):

1. improved the efficiency of the computing architecture, reducing memory usage by about 30% and improving computational efficiency by more than 30%.
2. fixed a major bug in the conditionally independent QTL mapping analysis.
3. solved the problem of insufficient precision of P-values (in versions before v0.2.8, too small P-values were displayed as 0). 
4. fixed a case where the heritability estimate would error out if it did not converge, and added information about convergence in the result output.



0. Table of contents

目录 OmiGA: a toolkit for Omics Genetic Analysis0. Table of contents1. Introduction1.1 Overview of OmiGA2. Getting Started2.1 Installation and Initial Setup2.1.1 Configuration and running for OmiGA2.2 quick starting3. Basic Options3.1 Input and Output3.2 Mode Selection3.3 Data Management4. QTL mapping4.1 Standard cis-QTL mapping4.2 Conditional independent cis-QTL mapping4.3 Interaction cis-QTL mapping4.4 cis-QTL multiple-testing4.5 Trans-QTL mapping4.6 GWAS (similar to global QTL mapping)5. Heritability estimation6. Contact Information

1. Introduction

1.1 Overview of OmiGA

Stay tuned for updates!

2. Getting Started

2.1 Installation and Initial Setup

2.1.1 Configuration and running for OmiGA

For a smooth and efficient installation process, we recommanded you downloading the Omiga software and saving it in your specified installation directory, known as soft_path. Additionally, please make sure to update soft_path and specify xzOmiGAname, which is the name of the Omiga vesrsion you are currently downloading.

    # Configure
    soft_path="/path/to/my/directory"# the directory of the OmiGA tar.xz /work/home/scau_zhangz_zwj/workPJ/test
    xzOmiGAname="OmiGA-build-v0.4.0-beta1.tar.xz" # the name of the OmiGA file
    

To ensure a successful operation, please directly copy and enter the following codes into your terminal. Upon successful completion, two environment variables linked to OmiGA will be located at the end of the ~/.bashrc file. 



‼️ Please ensure to run omiga --init  during the initial installation process to properly set up omiga.

    # Navigate to the directory and unzip the file
    cd ${soft_path} && tar -xf ${xzOmiGAname}
    
    # Extract the base name from the zip file
    base_name="${xzOmiGAname%.tar.xz}"
    
    # Remove old OMIGA_PATH if it exists in .bashrc
    sed -i '/OMIGA_PATH/d' ~/.bashrc
    sed -i '/\$OMIGA_PATH/d' ~/.bashrc
    
    # Add the new OMIGA_PATH to .bashrc
    echo "export OMIGA_PATH=\"${soft_path}/${base_name}/bin\"" >> ~/.bashrc
    echo "export PATH=\"\$OMIGA_PATH:\$PATH\"" >> ~/.bashrc
    
    # Source the .bashrc file to update the current shell environment
    source ~/.bashrc
    echo "OMIGA_PATH: $OMIGA_PATH"
    
    # Make the omiga binary executable
    chmod +x $OMIGA_PATH/omiga
    
    # Run OmiGA to check the version
    omiga --init
    

2.2 quick starting

Example:

    # Coming soon.



3. Basic Options

3.1 Input and Output



the same as those in tensorQTL

Required Arguments:

--genotype, --geno file

The prefix for the input PLINK binary files: "example", e.g. example.fam, example.bim and example.bed (see PLINK user manual for details).

    omiga --genotype ${global_path}/example

--phenotype, --pheno  file

Phenotypes in BED format (.bed,.bed.gz) or  plink phenotype format ( there must be a header row with first two entries 'FID' and "IID' or with a entry  "IID' ). If the --pheno parameter is used with a format1 file, the default value for --pheno-format should be set to bed and there is no need to provide the --pheno-annot parameter. However, if the --pheno argument is used with a format2 or format3 file, the --pheno-format option must be changed to plink and the --pheno-annot argument must be provided (except for gwas mode). The delimiters used in the file can be either tabs (\t) or spaces.

format1:  BED format (.bed, .bed.gz) 

The first row is header line, the first four columns are chromosome,  the physical location of the TSS, the physical location of the TSS +1, the name of the molecular phenotype. The fifth column begins to contain the values of the corresponding molecular phenotype for each sample.

    #Chr  start	  end      gene_id              SAMD1   SAME2     ...
    1     0	      1        ENSSSCG00000048769   0.906    2.555    ...
    1     22151	  22152    ENSSSCG00000037372   0.929    2.670    ...
    1     23781	  23782    ENSSSCG00000027257  -0.493    0.589    ...
    1     114010  114011   ENSSSCG00000049216  -0.771   -0.802    ...
    1     186748  186749   ENSSSCG00000029697   1.877   -1.641    ...
    1     212544  212545   ENSSSCG00000041543  -1.050   -1.050    ...
    ...

format2:  PLINK format (.txt,.txt.gz) [ the first row is header line; columns are family ID, individual ID and  (molecular) phenotypes ]

    FID   IID      ENSSSCG00000048769  ENSSSCG00000037372   ...
    0     SAMD1    0.906               0.929                ...
    0     SAME2    2.555               2.670                ...
    ...

format3: PLINK format (.txt, .txt.gz) [ the first row is header line; columns are individual ID and  (molecular) phenotypes ]

    IID      ENSSSCG00000048769  ENSSSCG00000037372   ...
    SAMD1    0.906               0.929                ...
    SAME2    2.555               2.670                ...
    ...

Examples: 

    omiga --mode cis --pheno ${format1} --pheno-format bed --geno ${geno} ...
    omiga --mode cis --pheno ${format2} --pheno-format plink --pheno-annot annotation.bed --geno ${geno} ...
    omiga --mode cis --pheno ${format3} --pheno-format plink --pheno-annot annotation.bed --geno ${geno} ...

--pheno-format string

Format of provided phenotypes (bed, plink). Default: bed

--pheno-annot file 

Phenotype annotation file. BED format (.bed,.bed.gz) 

annotation file (0-based coordinate system bed file): annotation.bed

The first four columns are chromosome,  the physical location of the TSS, the physical location of the TSS +1, the name of the molecular phenotype.

    #Chr	 start	   end       gene_id
    1	     0	       1  	     ENSSSCG00000048769
    1	     22151	   22152     ENSSSCG00000037372
    1	     23781	   23782	 ENSSSCG00000027257
    ...

--prefix string

Specify a prefix for the output file names.

--output-dir string

Specify the directory in which the output files will be generated.

 Optional Arguments:

--id-map file

Please provide a sample ID reference table with two columns. The first column should contain individual IDs matching the IIDs in the input PLINK binary files, and the second column should contain corresponding sample IDs for each tissue/organ matching the IIDs in the phenotype file. The table should have a header row, and tabs (\t) can be used as the delimiter. 

    IIDs	heart_id	    liver_id    	Gluteus_medius_id1	...
    AQ7400	SAMEA111507627	SAMEA111507628	SAMEA111507629	...
    AO7401	SAMEA111507631	SAMEA111507632	SAMEA111507633	...
    AQ7402	SAMEA111507635	SAMEA111507636	SAMEA111507637	...
    ...

--covariates file

The covariates file should be tab-delimited or spaced-delimited and can contain both discrete covariates (categorical factor with several levels) and quantitative covariates (continuous variables). The format is covariates x samples.

Input file format:

Each row represents a covariate, and each column represents a sample. [ the 1st column is covariates names, the 2-n columns are  individual IDs ]

          SAMEA104406691  SAMEA104406693   ...
    pc1    0.906               0.929                ...
    pc2    2.555               2.670                ...
    peer1  3.095               1.640                ...
    ...

--covar-transpose

This option must be specified if the provided covariates file is  in the format of samples x covariates.

--extract-covar-name string | list

Specify the covariates that will be used in the analysis.

--dcovar-name string | list

We can use name parameters to specify the discrete covariates.

    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} 
    --extract-covar-name pc1 pc2 peer1 peer2 year sex --dcovar-name year sex --output-dir ${output_dir}

--normalized-covar

Apply an inverse normal transform on all the covariates.

--phen-pc-covar int

selects the top n  principal components as covariates in phenotypic PCA.

--geno-pc-covar int

selects the top n  principal components as covariates in genotypic PCA.

--dprop-pc-covar float

OmiGA can automatically identify the inflection point for selecting the top n principal components in genotypic/phenotypic PCA. This is done by setting a threshold for the inflection point parameter. For example, if the threshold is set to "--dprop-pc-covar 0.001", it means that the percentage change in variance contribution between consecutive principal components is less than 0.001.



The function is designed to simplify the process of PCA and PC selection for you. OmiGA includes an automatic calculation feature for genotype PC and phenotype PC. You can use the options --phen-pc-covar, --dprop-pc-covar, and --geno-pc-covar individually or together. When used together, the function will select the minimum number of principal components. We suggest avoiding redundant information in your covariate file (e.g., phenotypic and/or genotypic PCs) when using the corresponding options here. 



--extract-pheno file

Specify which molecular phenotypes should be included in the analysis.

Input file format: file format 1：test.phenolist [ no header line; columns are (molecular) phenotype IDs ]

    ENSSSCG00000038931
    ENSSSCG00000026042
    ENSSSCG00000004172
    ENSSSCG00000004023
    ENSSSCG00000004336
    ENSSSCG00000051738
    ENSSSCG00000004008
    ENSSSCG00000050929
    ENSSSCG00000004253
    ...

--extract-pheno-name  string

Select only one molecular phenotype to include in the analysis.

    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} 
    --extract-covar-name pc1 pc2 peer1 peer2 year sex --extract-pheno-name ENSSSCG00000004336 --dcovar-name year sex --output-dir ${output_dir}

--exclude-pheno file

Specify which molecular phenotypes should be excluded in the analysis. the file format is same as the test.phenolist.

--exclude-pheno-name string | list

Specify one or more molecular phenotypes should be excluded in the analysis. This is the same as the --exclude-pheno function.

    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} 
    --extract-covar-name pc1 pc2 peer1 peer2 year sex --exclude-pheno-name ENSSSCG00000004008 ENSSSCG00000004253 ENSSSCG00000050929 --dcovar-name year sex --output-dir ${output_dir}



--pheno-group file

The parameter is only used for the cis or  cis_interaction run modes. Phenotype groups are represented in a header-less TSV file with two columns: phenotype_id and group_id. The delimiters used in the file can be either tabs (\t) or spaces.

    1:18696:22046:ENSSSCG00000037372 ENSSSCG00000037372
    1:18699:22046:ENSSSCG00000037372 ENSSSCG00000037372
    1:122275:122998:ENSSSCG00000029697 ENSSSCG00000029697
    1:123046:128198:ENSSSCG00000029697 ENSSSCG00000029697
    1:128290:131006:ENSSSCG00000029697 ENSSSCG00000029697

--grm file

If you want to provide your own GRMs, specify the directory where the additive or dominance GRM files are stored. The matrices must be in GCTA's grm_bin format. For additive GRMs, use file names with the suffix a.grm.bin and a.grm.id. For dominance GRMs, use file names with the suffix d.grm.bin and d.grm.id.



3.2 Mode Selection

Required Arguments:

--mode string

Specify the run mode. Options: her_est, cis, cis_independent, cis_interaction, cis_mt, trans, gwas. her_est: heritability estimation. cis: cis-QTL mapping. cis_independent: conditional independent QTL mapping. cis_interaction: interaction cis-QTL mapping. cis_mt: cis-QTL or interaction cis-QTL mpping multiple-testing. trans: trans-QTL mapping. gwas: genome-wide association study.

3.3 Data Management

Optional Arguments:

--maf-threshold float

Filter variants with minor allele frequency < maf_threshold.  Default: 0.01

--maf-threshold-interaction float

MAF threshold for cis_interaction mode, applied to lower and upper half of samples.  Default: 0.05

--mac-threshold int

Filter variants with minor allele count < mac_threshold. Default: 6

--rm-pheno-threshold float

We calculate the median value for each molecular phenotype across all samples. If the proportion of samples with values greater than the median exceeds a preset threshold, we retain the molecular phenotype for further analysis. Descriptive statistics will be outputted for all removed molecular phenotypes. Default: 0.1

Output: pre.cis_qtl.elp

Descriptive statistics, such as mean, standard deviation, minimum, 25th percentile, median, 75th percentile, and maximum values are calculated for the phenotype. The final column shows the count of unique values after removing duplicates from all samples.

    variable              mean      std     min     g25    median     g75     max    nuniqueall
    ENSGALG00010007874    0.009    0.892  -1.408  -0.473    0.384    0.384    2.35       7
    ENSGALG00010007339    -0.008   0.890  -1.548  -0.541    0.397    0.397    1.67       6
    ENSGALGB0010007592    0.058    0.592  -0.176  -0.176   -0.176   -0.176    1.19       3
    ENSGALG00010007003    0.011    0.229  -0.012  -0.012   -0.012   -0.012    2.35       2
    ...

--rm-collinear-covar float

When you need to eliminate multicollinearity, you can remove covariates with absolute value of correlation coefficients greater than the threshold you set. There is no default value established.

--het-rate-threshold float

Filter variants with a heterozygote rate greater than or equal to (≥) the specified threshold.  Default: 0.99

--chrom string | list

Specify one or more chromosome for analysis.

    omiga --mode cis --genotype ${geno} --chrom 2 --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir}
    omiga --mode cis --genotype ${geno} --chrom 1 2 3 --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir}

--seed int

Specify a random seed to ensure reproducibility of the results.

--multi-task list

The parameter is only used for the  trans or gwas run modes. You can use multiple task submissions to divide all (molecular) phenotypes into multiple groups and then complete task submissions group by group. This is done as follows:

    multask = 20
    for i  in  `seq ${multask}`
    do
    omiga --mode trans --genotype ${geno} --multi-task ${i} ${multask} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir} --thread 10
    done

--threads int

The program will automatically determine and set the number of threads running it. However, using too many threads may slow down the analysis if the complexity is not high enough. Users can customize the number of threads based on their specific needs. Default: the available maxinium number of CPU threads.

Examples: Data Management

    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir} --threads 10

--save-grm

When performing analyses that require the construction of additive or dominant kinship matrices, the generated matrices will be output in the same form as GCTA's grm_bin format.

--verbose

Verbose logs output.



4. QTL mapping

4.1 Standard cis-QTL mapping

--cis-window int

Cis-window size in bp for cis-QTL mapping. Default: 1,000,000

--qtl-map-model string

Statistical model for QTL mapping. Options: a, a+A,d+A, d+D, d+A+D, a+d+A+D. Default: a+A

a represents testing additive variants using a general linear model; a+A represents testing additive variants using a mixed linear model and correcting for the additive background polygenic effect; d+A represents testing dominant variants using a mixed linear model and correcting for the additive background polygenic effect; d+D represents testing dominant variants using a mixed linear model and correcting for the dominant background polygenic effect; a+d+A+D indicates simultaneous testing of both additive and dominant variants using a mixed linear model while also correcting for both types of background polygenic effects. 



Attention: d+A+D are also optional, but we recommend using them with caution, d+A+D represents testing dominant variants using a mixed linear model and correcting for both additive and dominant background polygenic effects.

--fdr float

FDR for QTL discovery. Default: 0.05

--preadj-covar

The phenotype will be pre-adjusted by the mean and covariates for QTL mapping.

--her-file file

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be a single file output from the her_est mode.

--her-file-multi files

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be multiple files output from the her_est mode.

--paed-file  string

Please provide the file path (*.jld2) for the pre-calculated Eigen factors used in the --qtl-map-model with two GRMs.

--multiple-testing string

Specify method used for multiple testing. Optional, OmiGA will select the most applicable option in the run-time. Options: acat, beta_approx, clipper.

--permutations int

Number of permutations. Required if --multiple-testing is specified.

--exact-map

Exact solution for QTL mapping. This will significantly reduce computational efficiency.

--write-top

Output permutation results for lead variants of each phenotypes without displaying full summary statistics.

--nominal-only

When this option is enabled, only nominal association tests are performed, and no permutation is conducted.

--calcu-variant-threshold

Calculate the variant level threshold.

--idul-max-iter int

The maximum number of iterations for idul.  Default: 20
--idul-converge float

Convergence criteria for idul algorithm.   Default: 1e-6

Example: Additive QTL

    # Linear mixed model (additive effects)
    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir}

Example: Dominant QTL

    # Linear mixed model (dominant effects)
    # 1. must first go to estimate global dominant heritability with '--h2-model Ag+Dg'
    # 2. use the output (*.her_est.txt.gz and *.jld2) from her_est as input
    her_file=${output_dir_of_her_est}/example.her_est.txt.gz
    preeigen=${output_dir_of_her_est}/_PreEigen.jld2
    omiga --mode cis --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --qtl-map-model d+A+D --her-file ${her_file} --paed-file ${preeigen} --output-dir ${output_dir}



4.2 Conditional independent cis-QTL mapping

--cis-file file

To use the cis_independent mode, you must have an output file (* .cis_qtl.txt.gz) that includes the q_val value.

--est-ind-h2

Perform SNP heritability estimation for cis_independent analysis.

--cis-window int

 Cis-window size in bp for cis-QTL mapping. Default: 1,000,000

--qtl-map-model string

Statistical model for QTL mapping. Options: a, a+A,d+A, d+D, d+A+D, a+d+A+D. Default: a+A

a represents testing additive variants using a general linear model; a+A represents testing additive variants using a mixed linear model and correcting for the additive background polygenic effect; d+A represents testing dominant variants using a mixed linear model and correcting for the additive background polygenic effect; d+D represents testing dominant variants using a mixed linear model and correcting for the dominant background polygenic effect; a+d+A+D indicates simultaneous testing of both additive and dominant variants using a mixed linear model while also correcting for both types of background polygenic effects. 



Attention: d+A+D are also optional, but we recommend using them with caution, d+A+D represents testing dominant variants using a mixed linear model and correcting for both additive and dominant background polygenic effects.

--fdr float

FDR for QTL discovery.  Default: 0.05

--preadj-covar

The phenotype will be pre-adjusted by the mean and covariates for QTL mapping.

--her-file file

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be a single file output from the her_est mode.

--her-file-multi files

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be multiple files output from the her_est mode.

--paed-file  string

Please provide the file path (*.jld2) for the pre-calculated Eigen factors used in the --qtl-map-model with two GRMs.

--exact-map

Exact solution for QTL mapping. This will significantly reduce computational efficiency.

--idul-max-iter int

The maximum number of iterations for idul.  Default: 20
--idul-converge float

Convergence criteria for idul algorithm.   Default: 1e-6

Example: Conditional independent cis-QTL mapping

    cis_file_fdr=${output_dir_of_cis_qtl}/example.cis_qtl.txt.gz
    omiga --mode cis_independent --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --cis-file ${cis_file_fdr} --qtl-map-model a+A --output-dir ${output_dir}



4.3 Interaction cis-QTL mapping

--interaction file

Interaction term(s): tab-delimited file, samples x term(s). Required for cis_interaction mode.

Input file format:

[ no header; the first column is IID, the 2-n column is interaction term  ]

    AQ7400  -0.34238
    AQ7401  -0.64669
    AQ7402  -0.23541
    AQ7612  0.651591
    AQ7613  0.679605
    AQ7614  0.657605

--cis-window int

 Cis-window size in bp for cis-QTL mapping. Default: 1,000,000

--no-add-interaction-to-covar

This is used to do not include the interaction term as a covariate when performing interaction cis-QTL mapping.

--normalized-interaction

Apply an inverse normal transform on the interaction term.

--fdr float

FDR for QTL discovery.  Default: 0.05

--preadj-covar

The phenotype will be pre-adjusted by the mean and covariates for QTL mapping.

--her-file file

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be a single file output from the her_est mode.

--her-file-multi files

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be multiple files output from the her_est mode.

--paed-file  string

Please provide the file path (*.jld2) for the pre-calculated Eigen factors used in the --qtl-map-model with two GRMs.

--multiple-testing string

Specify method used for multiple testing. Optional, OmiGA will select the most applicable option in the run-time. Options: acat, beta_approx, clipper.

--permutations int

Number of permutations. Required if --multiple-testing is specified.

--exact-map

Exact solution for QTL mapping. This will significantly reduce computational efficiency.

--write-top

Output permutation results for lead variants of each phenotypes without displaying full summary statistics.

--nominal-only

When this option is enabled, only nominal association tests are performed, and no permutation is conducted.

--calcu-variant-threshold

Calculate the variant level threshold.

--idul-max-iter int

The maximum number of iterations for idul.  Default: 20
--idul-converge float

Convergence criteria for idul algorithm.   Default: 1e-6

Example: Interaction cis-QTL mapping

    interaction=${your_interaction_term_path}/example.interactions_Pro1.txt
    omiga --mode cis_interaction --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --interaction ${interaction} --output-dir ${output_dir} --verbose --debug

4.4 cis-QTL multiple-testing



To avoid redundant calculations in cis  or  cis_interaction mode , you can utilize the cis_mt command with a different --multiple-testing options to recalculate the q-value for each gene.

--cis-file file

To use the cis_mt mode, you must have an output file (* .cis_qtl.txt.gz) from  the cis  or  cis_interaction mode.

--multiple-testing string

Specify method used for multiple testing. Optional, OmiGA will select the most applicable option in the run-time. Options: acat, beta_approx, clipper.

Example: cis-QTL mapping multiple testing

    OmiGA --mode cis_mt --prefix test --cis-file xx/example.cis_qtl.txt.gz --output-dir ./000-debug-test --multiple-testing acat



4.5 Trans-QTL mapping

--qtl-map-model string

Statistical model for QTL mapping. Options: a, a+A,d+A, d+D, d+A+D, a+d+A+D. Default: a+A

a represents testing additive variants using a general linear model; a+A represents testing additive variants using a mixed linear model and correcting for the additive background polygenic effect; d+A represents testing dominant variants using a mixed linear model and correcting for the additive background polygenic effect; d+D represents testing dominant variants using a mixed linear model and correcting for the dominant background polygenic effect; a+d+A+D indicates simultaneous testing of both additive and dominant variants using a mixed linear model while also correcting for both types of background polygenic effects. 



Attention: d+A+D are also optional, but we recommend using them with caution, d+A+D represents testing dominant variants using a mixed linear model and correcting for both additive and dominant background polygenic effects.

--preadj-covar

The phenotype will be pre-adjusted by the mean and covariates for QTL mapping.

--her-file file

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be a single file output from the her_est mode.

--her-file-multi files

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be multiple files output from the her_est mode.

--paed-file  string

Please provide the file path (*.jld2) for the pre-calculated Eigen factors used in the --qtl-map-model with two GRMs.

--exact-map

Exact solution for QTL mapping. This will significantly reduce computational efficiency.

--pval-threshold float

Output only significant phenotype-variant pairs with a p-value below threshold. Default: 10^{-5}

--idul-max-iter int

The maximum number of iterations for idul.  Default: 20
--idul-converge float

Convergence criteria for idul algorithm.   Default: 1e-6

Example: Trans-QTL mapping

    omiga --mode trans --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --cis-file ${cis_file_fdr} --output-dir ${output_dir}



4.6 GWAS (similar to global QTL mapping)

--qtl-map-model string

Statistical model for QTL mapping. Options: a, a+A,d+A, d+D, d+A+D, a+d+A+D. Default: a+A

a represents testing additive variants using a general linear model; a+A represents testing additive variants using a mixed linear model and correcting for the additive background polygenic effect; d+A represents testing dominant variants using a mixed linear model and correcting for the additive background polygenic effect; d+D represents testing dominant variants using a mixed linear model and correcting for the dominant background polygenic effect; a+d+A+D indicates simultaneous testing of both additive and dominant variants using a mixed linear model while also correcting for both types of background polygenic effects. 



Attention: d+A+D are also optional, but we recommend using them with caution, d+A+D represents testing dominant variants using a mixed linear model and correcting for both additive and dominant background polygenic effects.

--preadj-covar

The phenotype will be pre-adjusted by the mean and covariates for QTL mapping.

--her-file file

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be a single file output from the her_est mode.

--her-file-multi files

This file is essential for conducting QTL mapping using the options of --qtl-map-model d+A+D and/or --preadj-covar. It must be multiple files output from the her_est mode.

--paed-file  string

Please provide the file path (*.jld2) for the pre-calculated Eigen factors used in the --qtl-map-model with two GRMs.

--exact-map

Exact solution for QTL mapping. This will significantly reduce computational efficiency.

--pval-threshold float

Output only significant phenotype-variant pairs with a p-value below threshold. Default: 10^{-5}

--idul-max-iter int

The maximum number of iterations for idul. Default: 20
--idul-converge float

Convergence criteria for idul algorithm. Default: 1e-6

--lambda-snps int

The number of SNPs used to estimate λ. Default: 100000

--lambda-only 

Retrieve only the lambda values from the GWAS summary statistics, omitting the complete dataset.

Example: GWAS

    omiga --mode gwas --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir} --verbose --threads 8



5. Heritability estimation

--h2-model string

statistical model for heritability estimation. Options: Ag, Dg, Ag+Dg, At, Dt, Ag+Dt, Ac, Dc, Ag+Ac, Ag+Dc, At+Ac. Default: Ag. Ag: global additive GRM, Dg: global dominance GRM, At: trans additive GRM, Dt: trans dominance GRM, Ac: cis additive GRM, Dc: cis dominance GRM.

--h2-algo string

Algorithm for heritability estimation. Options: idul, minmax, em_aireml, idul_aireml.

--dpars float

Convergence parameter of heritability estimation. Default: 1e-5

--idul-max-iter int

The maximum number of iterations for idul. Default: 20
--idul-converge float

Convergence criteria for idul algorithm. Default: 1e-6

--off-low-rank-approx

Do not use low-rank matrix approximation for heritability estimation. The parameter can only be used if the --h2-model is Ag+Ac or At+Ac.

--approx-method string

Method for low-rank matrix approximation. The parameter can only be used if the --h2-model is Ag+Ac or At+Ac. Default: psvd 

--approx-rank float

Approximate rank for low-rank matrix approximation. OmiGA will set the rank as [?] of sample size if float type was provided. The parameter can only be used if the --h2-model is Ag+Ac or At+Ac.  Default: 0.05



Example: Estimate global, cis, trans heritability using additive genetic model (A)

    # Simultaneously estimate global, cis, trans heritability
    omiga --mode her_est --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir} --h2-model Ag --h2-algo em_aireml

Example: Estimate global heritability using additive & dominant genetic model (A+D)

    # Estimate global additive & dominant heritability
    omiga --mode her_est --genotype ${geno} --phenotype ${expr} --prefix ${prefix} --covariates ${covar} --output-dir ${output_dir} --h2-model Ag+Dg --h2-algo em_aireml 

6. Contact Information

For Software Development and Feature Requests:

- Jinyan Teng
  - :e-mail:: kingyan312@live.cn
- For discussions or queries related to software features and development, please contact Jinyan Teng directly.

For Software Usage Inquiries:

- Jinyan Teng
  - :e-mail:: kingyan312@live.cn
- Wenjing Zhang
  - :e-mail:: chinjcheung@163.com
- For assistance with software installation, configuration, and general usage, please contact either of the individuals listed above.


