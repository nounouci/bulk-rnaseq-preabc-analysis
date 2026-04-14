✅ VERSION CORRIGÉE (à copier tel quel)
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
--append-verify : reprend les transferts interrompus
--progress : affiche la progression
--ignore-existing : ignore les fichiers déjà copiés
> rsync.log 2>&1 & : envoie la sortie dans un log et lance en arrière-plan
Suivi du transfert
tail -f rsync.log

Permet de suivre la progression en temps réel.

Vérification des fichiers
ls ~/work/raw_data

Permet de vérifier que les fichiers ont bien été transférés
