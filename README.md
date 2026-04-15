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

Après le transfert des fichiers FASTQ vers le cluster, il est indispensable de vérifier que les fichiers n’ont pas été altérés ou corrompus. Cette vérification repose sur l’utilisation de checksums MD5.

---

## Principe

Un checksum MD5 est une empreinte numérique unique calculée à partir du contenu d’un fichier.

* Si le fichier est identique → le checksum est identique
* Si le fichier est modifié ou incomplet → le checksum est différent

La plateforme de séquençage fournit généralement un fichier contenant les checksums de référence.

---

## Fichiers disponibles

Deux fichiers de checksum peuvent être présents :

* `_check.md5` : contient les chemins complets (avec dossiers)
* `_checksum.md5` : contient uniquement les noms des fichiers

Dans ce projet, les fichiers FASTQ ont été copiés sans l’arborescence d’origine, il est donc nécessaire d’utiliser `_checksum.md5`.

---

## Procédure

### 1. Lister les fichiers présents

```bash
ls *.fastq.gz > mes_fichiers.txt
```

Cette commande crée un fichier contenant la liste des FASTQ présents dans le répertoire de travail.

---

### 2. Extraire les checksums correspondants

```bash
grep -Ff mes_fichiers.txt _checksum.md5 > subset.md5
```

Cette étape permet de filtrer le fichier de checksum pour ne conserver que les entrées correspondant aux fichiers effectivement présents.

---

### 3. Vérifier l’intégrité des fichiers

```bash
md5sum -c subset.md5
```

---

## Interprétation des résultats

Exemple de sortie :

```
P11-19_S19_L001_R1_001.fastq.gz: OK
P11-19_S19_L001_R2_001.fastq.gz: OK
...
```

* `OK` : le fichier est intact
* `FAILED` : le fichier est corrompu ou incomplet

---

## Gestion des erreurs

En cas de fichiers `FAILED`, les fichiers concernés doivent être supprimés puis retransférés.

Exemple :

```bash
rm P11-25_S25_L001_R2_001.fastq.gz
rm P11-25_S25_L002_R1_001.fastq.gz
```

Après retransfert, la vérification MD5 doit être relancée.

---
<img src="https://github.com/nounouci/bulk-rnaseq-preabc-analysis/blob/4e0bcc50c63d3a51d32e3a5d6c7ed35b37736fa4/Capture%20d%27%C3%A9cran%202026-04-14%20120421.png?raw=true" width="500">

## Résultat final

Une fois tous les fichiers validés, l’ensemble des FASTQ peut être utilisé en toute confiance pour les analyses downstream (FastQC, alignement, quantification).

---

## Remarque

Cette étape est critique car toute erreur dans les fichiers FASTQ peut compromettre l’ensemble de l’analyse RNA-seq.


###  Échantillons SLE utilisés pour l’analyse RNA-seq

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

````markdown
## Structure et compréhension des fichiers FASTQ

Les fichiers utilisés dans ce projet sont au format **FASTQ compressé (`.fastq.gz`)**, qui est le format standard pour les données de séquençage haut débit.

---

## Principe du format FASTQ

Un fichier FASTQ contient les séquences lues par le séquenceur ainsi que leur qualité.  
Chaque read est décrit sur **4 lignes** :

```text
@SEQ_ID
GATCGGAAGAGCACACGTCTGAACTCCAGTCAC
+
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
````

* Ligne 1 : identifiant de la séquence
* Ligne 2 : séquence nucléotidique (A, T, G, C)
* Ligne 3 : séparateur (`+`)
* Ligne 4 : qualité des bases (scores Phred encodés en ASCII)

---

## Compression

Les fichiers sont compressés au format `.gz` pour réduire leur taille :

* Avantage : gain de stockage
* Les outils bioinformatiques (FastQC, STAR, etc.) peuvent lire directement les fichiers compressés

---

## Structure des noms de fichiers

Exemple :

```
P11-19_S19_L001_R1_001.fastq.gz
```

Décomposition :

* `P11-19` : identifiant de l’échantillon
* `S19` : numéro interne de sample
* `L001` : lane de séquençage
* `R1` : read forward
* `001` : numéro technique

---

## Données paired-end

Les données sont en **paired-end**, ce qui signifie que chaque fragment est séquencé des deux côtés :

* `R1` : première lecture
* `R2` : seconde lecture

Ces deux fichiers doivent toujours être utilisés ensemble dans les analyses.

---

## Notion de lane

Un même échantillon peut être séquencé sur plusieurs lanes (ex : `L001`, `L002`) :

* Cela permet d’augmenter la profondeur de séquençage
* Les données issues de différentes lanes peuvent être :

  * soit utilisées séparément
  * soit concaténées avant l’analyse

---

## Exemple concret dans ce projet

Pour un échantillon donné :

```
P11-19_S19_L001_R1_001.fastq.gz
P11-19_S19_L001_R2_001.fastq.gz
P11-19_S19_L002_R1_001.fastq.gz
P11-19_S19_L002_R2_001.fastq.gz
```

Cela correspond à :

* 2 lanes (L001 et L002)
* 2 reads (R1 et R2)

---

## Utilisation dans l’analyse

Deux stratégies sont possibles :

### 1. Utilisation d’une seule lane (test)

Permet de tester rapidement le pipeline :

* plus rapide
* moins de données

---

### 2. Fusion des lanes (analyse finale)

Concaténation des fichiers :

```bash
cat *_L001_R1_*.fastq.gz *_L002_R1_*.fastq.gz > sample_R1.fastq.gz
cat *_L001_R2_*.fastq.gz *_L002_R2_*.fastq.gz > sample_R2.fastq.gz
```

* permet d’utiliser toutes les données
* recommandé pour l’analyse finale

---

## Remarque

Il est important de conserver la cohérence entre les fichiers :

* toujours associer R1 avec R2
* ne pas mélanger les échantillons
* vérifier l’intégrité avec MD5 avant toute analyse

```
```

---

````
## Contrôle qualité des données (FastQC)

Une analyse de qualité des reads a été réalisée à l’aide de l’outil FastQC sur un sous-ensemble de fichiers FASTQ afin de valider le bon fonctionnement du pipeline.

---

## Soumission du job sur le cluster

Le calcul a été exécuté via un script SLURM avec la commande :

```bash
sbatch fastqc_job.sh
````

---

## Suivi du job

Le job a été exécuté sur un nœud de calcul :

```bash
squeue -u $USER
```

Statut observé :

* `R` : job en cours d’exécution
* exécution sur le nœud `n008`

---

## Logs d’exécution

### Fichier de sortie standard

```bash
cat fastqc_35419443.log
```

Contenu :

```
Début FastQC : Tue Apr 14 15:09:59 CEST 2026
Analysis complete for P11-19_S19_L001_R1_001.fastq.gz
Analysis complete for P11-19_S19_L001_R2_001.fastq.gz
Analysis complete for P11-20_S20_L001_R1_001.fastq.gz
Analysis complete for P11-20_S20_L001_R2_001.fastq.gz
Fin FastQC : Tue Apr 14 15:12:09 CEST 2026
```

---

### Fichier d’erreur (suivi de progression)

```bash
cat fastqc_35419443.err
```

Ce fichier contient la progression de l’analyse :

```
Started analysis of ...
Approx 5% complete ...
...
Approx 95% complete ...
```

Cela confirme que FastQC a traité les fichiers correctement.

---

## Résultat

* Les 4 fichiers FASTQ test ont été analysés avec succès
* Durée d’exécution : environ 2 minutes
* Aucun message d’erreur critique

Les rapports FastQC sont disponibles dans le dossier de sortie :

```
~/work/fastqc/
```

---

## Conclusion

Cette étape valide :

* le bon fonctionnement de FastQC sur le cluster
* la configuration correcte du script SLURM
* la disponibilité des données FASTQ

Le pipeline peut maintenant être appliqué à l’ensemble des échantillons.

```

---
```



---

````
## Agrégation des résultats de qualité (MultiQC)

Après l’analyse individuelle des fichiers FASTQ avec FastQC, une étape d’agrégation des résultats a été réalisée à l’aide de l’outil **MultiQC**.

---

## Principe

MultiQC permet de regrouper les rapports issus de FastQC pour plusieurs échantillons en un seul fichier interactif.

Cela permet :

- une visualisation globale de la qualité des données
- une comparaison entre échantillons
- une détection rapide des anomalies communes

---

## Script SLURM utilisé

```bash
#!/bin/bash
#SBATCH --job-name=multiqc
#SBATCH --output=multiqc_%j.log
#SBATCH --error=multiqc_%j.err
#SBATCH --partition=workq
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G
#SBATCH --time=01:00:00

echo "Début MultiQC : $(date)"

module load bioinfo/MultiQC

cd ~/work/fastqc

multiqc . -o ../multiqc

echo "Fin MultiQC : $(date)"
````

---

## Résultat

Un rapport HTML unique a été généré :

```
~/work/multiqc/multiqc_report.html
```

Ce rapport permet d’explorer les résultats de qualité de l’ensemble des échantillons.

---

## Interprétation des résultats

Les principales observations sont les suivantes :

* Nombre de reads par échantillon : ~15 millions → cohérent
* Contenu en GC : ~50% → conforme aux attentes biologiques
* Duplication : ~30–38% → acceptable en RNA-seq
* Présence d’adapters : détectée (notamment sur R2)
* Qualité des bases : diminution en fin de reads

---

## Conclusion

L’analyse MultiQC met en évidence :

* une bonne qualité globale des données
* la présence d’adapters et de bases de faible qualité

Ces observations justifient la réalisation d’une étape de trimming avant l’alignement.

---

````



---

````
## Trimming des reads (TrimGalore)

Suite aux résultats obtenus avec FastQC et MultiQC, plusieurs anomalies ont été identifiées dans les données :

- Présence de séquences d’adapters (notamment dans les reads R2)
- Diminution de la qualité des bases en fin de reads (extrémité 3')
- Variations du contenu en bases le long des reads

Afin de corriger ces problèmes et d’améliorer la qualité des données, une étape de trimming a été mise en place.

---

## Objectifs du trimming

Le trimming permet de :

- supprimer les séquences d’adapters
- éliminer les bases de faible qualité
- retirer les reads trop courts après nettoyage

Cette étape est essentielle pour améliorer :

- la qualité de l’alignement
- la précision de la quantification
- la fiabilité des analyses downstream

---

## Outils utilisés

Le trimming est réalisé avec :

- **TrimGalore (v0.6.10)**  
- reposant sur **Cutadapt (v5.0)**

---

## Script SLURM utilisé

```bash
#!/bin/bash
#SBATCH --job-name=trim_galore
#SBATCH --output=trim_%j.log
#SBATCH --error=trim_%j.err
#SBATCH --partition=workq
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=02:00:00

echo "Début trimming : $(date)"

module load bioinfo/Cutadapt/5.0
module load bioinfo/TrimGalore/0.6.10

cd ~/work/raw_data
mkdir -p ../trimmed

trim_galore \
  --paired \
  --quality 20 \
  --length 20 \
  --cores 4 \
  -o ../trimmed \
  P11-19_S19_L001_R1_001.fastq.gz P11-19_S19_L001_R2_001.fastq.gz \
  P11-20_S20_L001_R1_001.fastq.gz P11-20_S20_L001_R2_001.fastq.gz

echo "Fin trimming : $(date)"
````

---

## Paramètres utilisés

* `--paired` : données paired-end (R1/R2)
* `--quality 20` : suppression des bases avec une qualité inférieure à Q20
* `--length 20` : suppression des reads trop courts (< 20 bp)
* `--cores 4` : utilisation de 4 threads
* `-o` : dossier de sortie des fichiers trimmed

---

## Statut de l’analyse

Le trimming est actuellement en cours d’exécution sur le cluster de calcul.

Les fichiers générés seront :

```
*_val_1.fq.gz
*_val_2.fq.gz
```

ainsi que des rapports :

```
*_trimming_report.txt
```

---

## Étape suivante

Une fois le trimming terminé, un nouveau contrôle qualité sera réalisé :

* FastQC sur les fichiers trimmed
* agrégation des résultats avec MultiQC

Cela permettra de vérifier :

* la suppression des adapters
* l’amélioration de la qualité des reads
* la qualité globale des données avant alignement

```
```




