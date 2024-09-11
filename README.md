# OmiGA [![GitHub issues](https://img.shields.io/github/issues/SCAU-AnimalGenetics/OmiGA?color=green)](https://github.com/SCAU-AnimalGenetics/OmiGA/issues/new) [![](https://img.shields.io/badge/Release-v0.4.0-important.svg)](https://china.scidb.cn/download?fileId=25cf1a4daf14bbd706779d65af39297d&shortLink=2mAJFv&traceId=kingyan312@live.cn) [![GitHub Wiki](https://img.shields.io/badge/GitHub-Wiki-yellow)](https://github.com/SCAU-AnimalGenetics/OmiGA/wiki)<br>

## A Toolkit For [O](https://github.com/SCAU-AnimalGenetics/OmiGA/)mics [G](https://github.com/SCAU-AnimalGenetics/OmiGA/)enetic [A](https://github.com/SCAU-AnimalGenetics/OmiGA/)nalysis<br>

## Introduction <br>
**OmiGA** is a toolkit designed to ultra-efficient genetic analysis for omics data accounting for population structure and relatedness. The toolkit have two essential modules: (1) LMM-based methods for mapping molecular QTL (cis-QTL, content-dependent cis-QTL, trans-QTL, global QTL) with additive and/or non-additive effects; and (2) additive and/or non-additive (cis, trans, and global) heritability partitioning for large-scale molecular phenotypes. **OmiGA** requires ==three fundamental input data components==: (1) complete or imputed genotype data for all genetic variants; (2) a numerical matrix recording phenotype values with annotations specifying the genomic locations (e.g., transcription start sites) for all molecular traits; and (3) covariates accounting for the batch effects and related hidden confounders within the dataset.
