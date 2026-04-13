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

Les données analysées correspondent à des données de RNA-seq bulk (paired-end) au format FASTQ (.fastq.gz).

Les fichiers bruts étaient initialement stockés sur le réseau interne de l’équipe (INSERM) au chemin suivant :

\\infinity-share.inserm.lan\INFY_Eq_Guery\Public\sequencing_CRCT P11 librairies\PreABC_FO_SLE

Ils ont ensuite été transférés vers le cluster de calcul GenoToul pour analyse.

Les données ne sont pas incluses dans ce dépôt.

## Transfert des données
Les fichiers RNA-seq ont été transférés depuis un stockage local vers le cluster GenoToul à l’aide de la commande suivante :

```bash
rsync -avhz --progress *.fastq.gz \
neddassouqu@genobioinfo.toulouse.inrae.fr:/home/neddassouqu/work/raw_data/
```
Pipeline d’analyse

Le pipeline comprend les étapes suivantes :

Contrôle qualité des lectures (FastQC)
Alignement sur le génome de référence (STAR)
Quantification des lectures (featureCounts)
Analyse différentielle (DESeq2)
Analyses fonctionnelles (GSEA, IPA)
Organisation du projet

scripts/ Scripts bash pour le pipeline d’analyse
results/ Résultats (non versionnés)
data/ Données (non incluses dans le dépôt)

Outils utilisés
FastQC
STAR
featureCounts
DESeq2 (R)
GSEA / IPA
Auteur

Noufissa Eddassouqui
Stage M1 Bioinformatique
INFINITY – INSERM UMR1291, Toulouse
