# Organisation du dépôt

Ce dépôt est destiné à présenter le workflow, les scripts, les paramètres importants, quelques figures finales et de petites tables de résultats.

## À versionner

- scripts Bash, R et Python
- fichiers de configuration légers
- documentation du pipeline
- petites tables de synthèse
- figures finales utiles à l’interprétation

## À ne pas versionner

- FASTQ, BAM, CRAM et index lourds
- objets R volumineux
- répertoires `work/` et sorties Nextflow
- archives ZIP de FastQC, MultiQC, STAR, Qualimap, RSeQC, Salmon et nf-core
- résultats intermédiaires régénérables

Les sorties lourdes doivent être conservées sur un espace de stockage dédié. GitHub sert ici à rendre l’analyse lisible et reproductible, pas à archiver l’ensemble des produits du pipeline.
