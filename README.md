# <img src="https://zwj-typora.oss-cn-shanghai.aliyuncs.com/GWAS/202409121006707.ico" width="80" height="80"> **OmiGA** [![GitHub issues](https://img.shields.io/github/issues/SCAU-AnimalGenetics/OmiGA?color=green)](https://github.com/SCAU-AnimalGenetics/OmiGA/issues/new) [![](https://img.shields.io/badge/Release-v0.4.0-important.svg)](https://china.scidb.cn/download?fileId=25cf1a4daf14bbd706779d65af39297d&shortLink=2mAJFv&traceId=kingyan312@live.cn) [![GitHub Wiki](https://img.shields.io/badge/GitHub-Wiki-blue)](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki) [![](https://img.shields.io/badge/license-GPL3.0-yellow.svg)](https://github.com/SCAU-AnimalGenetics/OmiGA/blob/main/LICENSE)<br>

## A Toolkit for [Omi](https://github.com/SCAU-AnimalGenetics/OmiGA/)cs [G](https://github.com/SCAU-AnimalGenetics/OmiGA/)enetic [A](https://github.com/SCAU-AnimalGenetics/OmiGA/)nalysis<br>

## Introduction <br>
[**OmiGA**](https://china.scidb.cn/download?fileId=32b906689ab7c3cd05dbd0a787547b2d&username=kingyan312@live.cn&traceId=kingyan312@live.cn) is a toolkit designed to ultra-efficient genetic analysis for omics data accounting for population structure and relatedness. The toolkit have two essential modules: (1) LMM-based methods for mapping molecular QTL (cis-QTL, content-dependent cis-QTL, trans-QTL, global QTL) with additive and/or non-additive effects; and (2) additive and/or non-additive (cis, trans, and global) heritability partitioning for large-scale molecular phenotypes. **OmiGA** requires three fundamental input data components: (1) complete or imputed genotype data for all genetic variants; (2) a numerical matrix recording phenotype values with annotations specifying the genomic locations (e.g., transcription start sites) for all molecular traits; and (3) covariates accounting for the batch effects and related hidden confounders within the dataset. For detailed documentation, please click [**Here**](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki).


## Contents
<!-- TOC updateOnSave:false -->

- [1.Introduction](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#1-introduction)
  - [1.1 Overview of OmiGA](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#11-overview-of-omiga)
- [2. Getting Started](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#2-getting-started)
  - [2.1 Installation and Initial Setup](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#21-installation-and-initial-setup)
    - [2.1.1 Configuration and running for OmiGA](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#211-configuration-and-running-for-omiga)
  - [2.2 quick starting](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#22-quick-starting)
- [3. Basic Options](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#3-basic-options)
  - [3.1 Input and Output](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#31-input-and-output)
  - [3.2 Mode Selection](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#32-mode-selection)
  - [3.3 Data Management](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#33-data-management)
- [4. QTL mapping](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#4-qtl-mapping)
  - [4.1 Standard cis-QTL mapping](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#41-standard-cis-qtl-mapping)
  - [4.2 Conditional independent cis-QTL mapping](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#42-conditional-independent-cis-qtl-mapping)
  - [4.3 Interaction cis-QTL mapping](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#43-interaction-cis-qtl-mapping)
  - [4.4 cis-QTL multiple-testing](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#44-cis-qtl-multiple-testing)
  - [4.5 Trans-QTL mapping](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#45-trans-qtl-mapping)
  - [4.6 GWAS(similar to global QTL mapping)](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#46-gwas-similar-to-global-qtl-mapping)
- [5. Heritability estimation](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#5-heritability-estimation)
- [6. Contact Information](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki#6-contact-information)
 
<!-- /TOC -->

