# Bulk RNA-seq analysis – PreABC vs FO B cells

## Contexte
Ce projet est réalisé dans le cadre d’un stage de bioinformatique au sein de l’unité INFINITY (INSERM UMR1291, Toulouse).  
Il vise à analyser des données de RNA-seq bulk afin de caractériser les différences transcriptionnelles entre différentes populations de lymphocytes B.

## Objectifs
- Identifier les gènes différentiellement exprimés (DEG) entre les populations PreABC et FO  
- Caractériser les signatures transcriptomiques associées  
- Étudier l’expression des gènes liés au chromosome X  
- Réaliser des analyses d’enrichissement fonctionnel (GSEA, IPA)  

## Données
Les données utilisées sont des données de RNA-seq bulk (paired-end) au format FASTQ (.fastq.gz), générées et hébergées sur la plateforme GenoToul.

Les données brutes ne sont pas incluses dans ce dépôt.

## Pipeline d’analyse
Le pipeline comprend les étapes suivantes :

1. Contrôle qualité des lectures (FastQC)  
2. Alignement sur le génome de référence (STAR)  
3. Quantification des lectures (featureCounts)  
4. Analyse différentielle (DESeq2)  
5. Analyses fonctionnelles (GSEA, IPA)  

## Organisation du projet

scripts/        Scripts bash pour le pipeline d’analyse  
results/        Résultats (non versionnés)  
data/           Données (non incluses dans le dépôt)  

## Outils utilisés
- FastQC  
- STAR  
- featureCounts  
- DESeq2 (R)  
- GSEA / IPA  

## Auteur
Noufissa Eddassouqui  
Stage M1 Bioinformatique  
INFINITY – INSERM UMR1291, Toulouse
