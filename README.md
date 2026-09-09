# Bulk RNA-seq analysis – PreABC vs Neg B cells

## Contexte

Ce dépôt documente une analyse RNA-seq bulk réalisée dans le cadre d’un stage de Master 1 à l’unité INFINITY. L’objectif est de comparer les profils transcriptomiques de cellules B PreABC et Neg issues de souris SLE.

## Données

Les données correspondent à du RNA-seq bulk paired-end Illumina. L’analyse utilise 8 échantillons :

- 4 échantillons PreABC
- 4 échantillons Neg
- échantillons appariés par souris

Les fichiers FASTQ, BAM, objets R volumineux et sorties intermédiaires générées par les outils ne sont pas versionnés dans Git. Ils doivent être conservés sur un espace de stockage adapté et peuvent être régénérés à partir du pipeline.

## Workflow d’analyse

1. Vérification MD5 des FASTQ
2. Contrôle qualité initial avec FastQC et MultiQC
3. Trimming avec Trim Galore
4. Suppression des queues polyA/polyT avec Cutadapt
5. Contrôle qualité après nettoyage
6. Construction d’un index STAR adapté aux reads de 150 bp
7. Alignement avec STAR
8. Contrôle qualité des BAM avec Qualimap et RSeQC
9. Quantification transcriptomique avec Salmon
10. Import des quantifications avec tximport
11. Analyse différentielle avec DESeq2 selon un design apparié : `~ mouse + condition`

## Résultats principaux

Les contrôles RSeQC indiquent une librairie stranded reverse, compatible avec l’option Salmon `ISR`.

Les taux d’alignement STAR sont élevés, autour de 87–89 % de reads alignés de façon unique.

La quantification Salmon directe sur l’index gentrome/decoy présente un taux de mapping plus faible, autour de 32–42 %, probablement en raison d’un grand nombre de fragments rejetés comme decoy ou pour score d’alignement insuffisant.

L’analyse finale retenue repose sur STAR-Salmon puis DESeq2 avec un design apparié.

## Organisation recommandée du dépôt

```text
bulk-rnaseq-preabc-analysis/
├── README.md
├── LICENSE
├── .gitignore
├── scripts/          # Bash, R et Python du pipeline
├── docs/             # documentation et notes de workflow
├── figures/          # quelques figures finales utiles
└── results_summary/  # petites tables récapitulatives
```

Les gros résultats intermédiaires (FastQC, MultiQC, STAR, Qualimap, RSeQC, Salmon, nf-core, etc.) ne doivent pas être commités sous forme d’archives ZIP.

## Reproductibilité

Le dépôt a vocation à contenir le code, les paramètres, les petites tables utiles et la documentation nécessaire pour comprendre et reproduire l’analyse, sans dupliquer les données brutes ni les sorties lourdes du pipeline.
