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
```

### Explication des options

- `nohup` : exécute la commande en arrière-plan sans interruption  
- `rsync` : outil de transfert de fichiers  
- `-a` : mode archive (préserve permissions et structure)  
- `-v` : mode verbeux (affichage détaillé)  
- `-h` : affichage lisible des tailles  
- `--partial` : conserve les fichiers incomplets en cas d’interruption  
- `--append-verify` : reprend les transferts interrompus avec vérification  
- `--progress` : affiche la progression du transfert  
- `--ignore-existing` : ignore les fichiers déjà copiés  
- `> rsync.log 2>&1 &` : redirige la sortie vers un fichier log et lance le processus en arrière-plan  

---

### Suivi du transfert

```bash
tail -f rsync.log
```

Permet de suivre la progression en temps réel.

---

### Vérification des fichiers

```bash
ls ~/work/raw_data
```

Permet de vérifier que les fichiers ont bien été transférés.
## Vérification de l’intégrité des données (MD5)

Après le transfert des fichiers FASTQ, l’intégrité des données a été vérifiée à l’aide des checksums MD5 fournis.

Le fichier `_checksum.md5`, contenant les empreintes MD5 associées aux fichiers, a été utilisé.

---

### Préparation du fichier de vérification

Étant donné que seuls certains fichiers ont été transférés, un sous-ensemble des checksums a été généré :

```bash
ls *.fastq.gz > mes_fichiers.txt
grep -Ff mes_fichiers.txt _checksum.md5 > subset.md5
```


### Vérification des fichiers
```bash
md5sum -c subset.md5
```
Tous les fichiers utilisés pour l’analyse sont valides (OK), confirmant l’absence de corruption lors du transfert.
### Remarque

Le fichier _check.md5, contenant des chemins complets, n’a pas été utilisé car il ne correspondait pas à l’organisation loc


## Échantillons SLE utilisés pour l’analyse RNA-seq

Les analyses ont été réalisées sur des lymphocytes B issus de souris SLE, comprenant deux conditions expérimentales : Pré-ABC et Neg.  
Chaque paire d’échantillons correspond à une même souris, permettant une comparaison directe entre les deux conditions.

Le tableau ci-dessous présente les informations associées aux échantillons utilisés.

---

## Table des échantillons SLE (PreABC vs Neg)

| Souris   | Condition | Sample names          | ARN extrait & QC | Nombre de cellules | Échantillon n° | ID librairie |
|---------|----------|-----------------------|------------------|--------------------|----------------|--------------|
| SLE 1567 | Pré-ABC | Pré-ABC CD11c+CD11b+ | oui              | 312000             | 19             | P11-19       |
| SLE 1567 | Neg     | Neg                   | oui              | 500000             | 20             | P11-20       |
| SLE 1569 | Pré-ABC | Pré-ABC               | oui              | 462000             | 21             | P11-21       |
| SLE 1569 | Neg     | Neg                   | oui              | 500000             | 22             | P11-22       |
| SLE 1671 | Pré-ABC | Pré-ABC               | oui              | 186000             | 39             | P11-23       |
| SLE 1671 | Neg     | Neg                   | oui              | 500000             | 40             | P11-24       |
| SLE 2142 | Pré-ABC | Pré-ABC               | oui              | 259000             | 43             | P11-25       |
| SLE 2142 | Neg     | Neg                   | oui              | 500000             | 44             | P11-26       |
