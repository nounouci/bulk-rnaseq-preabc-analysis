# Bulk RNA-seq analysis – PreABC vs FO B cells

## Contexte

Ce projet a été réalisé dans le cadre d’un stage de bioinformatique au sein de l’unité INFINITY (INSERM UMR1291, Toulouse).

L’objectif est d’analyser des données de RNA-seq bulk afin de comparer les profils transcriptionnels entre différentes populations de lymphocytes B, notamment les populations PreABC et FO.

---

## Objectifs

- Identifier les gènes différentiellement exprimés (DEG) entre les populations PreABC et FO  
- Décrire les signatures transcriptomiques spécifiques  
- Étudier l’expression des gènes liés au chromosome X  
- Réaliser des analyses d’enrichissement fonctionnel (GSEA, IPA)  

---

## Données

Les données utilisées correspondent à du RNA-seq bulk paired-end (Illumina) au format compressé `.fastq.gz`.

### Localisation initiale

### Exemples de fichiers


P11-19_S19_L001_R1_001.fastq.gz
P11-19_S19_L001_R2_001.fastq.gz
P11-20_S20_L001_R1_001.fastq.gz
P11-20_S20_L001_R2_001.fastq.gz
...
P11-26_S26_L002_R2_001.fastq.gz


Les données ne sont pas incluses dans ce dépôt en raison de leur volume.

---

## Transfert des données

Les fichiers FASTQ ont été transférés vers le cluster GenoToul dans le répertoire :


~/work/raw_data/


### Commande utilisée

```bash
nohup rsync -avh --partial --append-verify --progress --ignore-existing *.fastq.gz \
neddassouqu@genobioinfo.toulouse.inrae.fr:/home/neddassouqu/work/raw_data/ \
> rsync.log 2>&1 &
Suivi du transfert
tail -f rsync.log
Vérification des fichiers transférés
ls ~/work/raw_data

Noufissa Eddassouqui
Stage M1 Bioinformatique
INFINITY – INSERM UMR1291, Toulouse
