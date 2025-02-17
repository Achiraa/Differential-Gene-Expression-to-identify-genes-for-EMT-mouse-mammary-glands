# Differential Gene Expression Analysis
## This repository contains R scripts and data for performing differential gene expression analysis using DESeq2 and various visualization tools.

## Prerequisites
Ensure you have the following R packages installed:

install.packages(c("DESeq2", "tidyverse", "BiocGenerics", "S4Vectors", "stats4", "ggplot2", "ggrepel", "EnhancedVolcano", "gplots", "ComplexHeatmap", "circlize", "RColorBrewer", "cummeRbund"))

## SetUp
Set the working directory to the location of your data files:

setwd("C:/geo_data/Stromal")

## Data
Sample Information: Stromaltomatosource.csv

Gene Counts: Stromaltomato.csv

## Steps
## 1. Load Libraries

library(DESeq2)

library(tidyverse)

library(BiocGenerics)

library(S4Vectors)

library(stats4)

library(ggplot2)

library(ggrepel)

library(EnhancedVolcano)

library(gplots)

library(ComplexHeatmap)

library(circlize)

library(RColorBrewer)

library(cummeRbund)

## 2. Read Sample Info

sdata = read.csv("Stromaltomatosource.csv")

View(sdata)

## 3. Read Gene Counts

rw = read.csv("Stromaltomato.csv")

rw1 = rw[,-1]

row.names(rw1) = make.names(rw[,1], unique = TRUE)

View(rw1)

## 4. Check Consistency

all(colnames(rw1) %in% rownames(sdata))

all(colnames(rw1) == rownames(sdata))

## 5. Convert Count Data

dds_gene = DESeqDataSetFromMatrix(countData = rw1, colData = sdata, design = ~ Treatment)

## 6. Filter Genes

keepgene = rowSums(counts(dds_gene)) >= 10

dds_gene1 = dds_gene[keepgene,]

## 7. Normalize Counts

dds_genenew = estimateSizeFactors(dds_gene1)

sizeFactors(dds_genenew)

## 8. Variance Stabilization

rld = rlog(dds_genenew, blind = TRUE)

## 9. Differential Expression Analysis

newdds_gene = DESeq(dds_genenew)

newdds_gene$Treatment = relevel(dds_genenew$Treatment, ref = "Untreated")

resultss = results(newdds_gene)

## 10. Filter Significant Genes

result.05 = results(newdds_gene, alpha = 0.05)

resultsort = result.05[order(result.05$pvalue),]

write.csv(resultsort, file = "ALLstromaltomatoGenes.csv")

significant_genes = resultsort[complete.cases(resultsort) & resultsort$pvalue < 0.05, ]

write.csv(significant_genes, file = "Significant_tomato_genes.csv")


## Plots

## PheatMap
![Luminalheatmapfinal](https://github.com/Achiraa/Differential-Gene-Expression/assets/114616203/743e4623-a024-42b4-a780-1e8ae77cc0c6)

## MA Plot
![MAplot0 05](https://github.com/Achiraa/Differential-Gene-Expression/assets/114616203/b3a98eb1-67a8-41f4-b2c9-189fbe5680ad)

## Volcano Plot
![volcano](https://github.com/Achiraa/Differential-Gene-Expression/assets/114616203/2c247202-db2d-4616-8901-dcff77725f35)

## Top DEGS 
![top20DEGS](https://github.com/Achiraa/Differential-Gene-Expression/assets/114616203/99ff38de-0fdd-41e4-b2c3-d59b41079888)

## Waterfall Plot
![Waterfall_BMP](https://github.com/Achiraa/Differential-Gene-Expression/assets/114616203/80269909-1b35-4d95-9a04-5d58ef7ab4e2)


