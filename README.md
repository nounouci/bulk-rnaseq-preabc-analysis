# Bulk RNA-seq analysis – PreABC vs Neg B cells

## Contexte

Ce dépôt documente une analyse RNA-seq bulk réalisée dans le cadre d’un stage de Master 1 à l’unité INFINITY. L’objectif est de comparer les profils transcriptomiques de cellules B PreABC et Neg issues de souris SLE.

## Données

Les données correspondent à du RNA-seq bulk paired-end Illumina. Les fichiers FASTQ, BAM, RDS et autres fichiers volumineux ne sont pas inclus dans ce dépôt.

L’analyse utilise 8 échantillons :
- 4 échantillons PreABC
- 4 échantillons Neg
- appariés par souris

## Organisation du pipeline

Le workflow comprend les étapes suivantes :

1. Vérification MD5 des fichiers FASTQ
2. Contrôle qualité initial avec FastQC et MultiQC
3. Trimming des adapters et bases de faible qualité avec Trim Galore
4. Suppression des queues polyA/polyT avec Cutadapt
5. Nouveau contrôle qualité après nettoyage
6. Construction d’un index STAR adapté aux reads 150 bp
7. Alignement des reads avec STAR
8. Contrôle qualité des BAM avec Qualimap et RSeQC
9. Quantification transcriptomique avec Salmon
10. Import des quantifications avec tximport
11. Analyse différentielle avec DESeq2 selon un design apparié : `~ mouse + condition`

## Résultats principaux

Les contrôles RSeQC indiquent une librairie stranded reverse, compatible avec l’option Salmon `ISR`.

Les taux d’alignement STAR sont élevés, autour de 87–89 % de reads alignés de façon unique.

La quantification Salmon directe sur l’index gentrome/decoy présente un taux de mapping plus faible, autour de 32–42 %, probablement en raison d’un grand nombre de fragments rejetés comme decoy ou pour score d’alignement insuffisant.

L’analyse finale retenue repose sur STAR-Salmon puis DESeq2 avec un design apparié.

## Organisation du dépôt

```text
scripts/
  Scripts Bash, R et Python utilisés pour le pipeline

docs/
  Notes explicatives et documentation du workflow

figures_for_report/
  Figures ou schémas destinés au rapport

README.md
  Présentation générale du projet
