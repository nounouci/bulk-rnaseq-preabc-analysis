# Bulk RNA-seq analysis – PreABC vs FO B cells

## Contexte

Ce projet a été réalisé dans le cadre d’un stage de bioinformatique au sein de l’unité INFINITY (INSERM UMR1291, Toulouse).

L’objectif de ce travail est d’analyser des données de RNA-seq bulk afin de comparer les profils transcriptionnels entre différentes populations de lymphocytes B, en particulier les populations PreABC et FO.

Cette analyse s’inscrit dans un contexte d’étude des mécanismes moléculaires régulant la différenciation et la fonction des cellules B, avec un intérêt particulier pour l’expression génique et les signatures transcriptionnelles spécifiques.

---

## Objectifs

- Identifier les gènes différentiellement exprimés (DEG) entre les populations PreABC et FO  
- Décrire les signatures transcriptomiques spécifiques à chaque population  
- Étudier l’expression des gènes liés au chromosome X  
- Réaliser des analyses d’enrichissement fonctionnel (GSEA, IPA)  

---

## Données

Les données utilisées correspondent à du RNA-seq bulk paired-end (Illumina), au format compressé `.fastq.gz`.

### Localisation initiale

Les fichiers étaient initialement stockés sur un serveur interne :

### Organisation des fichiers

Chaque échantillon est représenté par deux fichiers :

- R1 : reads forward  
- R2 : reads reverse  

Exemple :


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
Explication des options
nohup : exécute la commande en arrière-plan sans interruption
rsync : outil de transfert de fichiers
-a : mode archive
-v : mode verbeux
-h : tailles lisibles
--partial : conserve les fichiers incomplets
--append-verify : reprend les transferts interrompus avec vérification
--progress : affiche la progression
--ignore-existing : ignore les fichiers déjà copiés
> rsync.log 2>&1 & : redirige la sortie vers un log et lance en arrière-plan
Suivi du transfert
tail -f rsync.log

Permet de suivre la progression en temps réel.

Vérification des fichiers
ls ~/work/raw_data

Permet de vérifier que les fichiers ont bien été transférés.
