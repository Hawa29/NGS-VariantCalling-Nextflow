
# Analyse RNA-seq et détection de variants génétiques avec Nextflow

## Présentation du projet

Projet réalisé individuellement dans le cadre du **Master 2 Bioinformatique et Biologie des Systèmes à l'Université de Toulouse (2025–2026)**.

Ce projet porte sur l'analyse de données de séquençage RNA-seq et la détection de variants génétiques (**SNPs et indels**) à l'aide du pipeline **nf-core/rnavar**, orchestré par **Nextflow** et exécuté sur le cluster de calcul haute performance **Genotoul**.

L'objectif était de mettre en œuvre un workflow bioinformatique reproductible, depuis la récupération et la préparation des données jusqu'à l'exécution du pipeline et l'examen des résultats de détection de variants.

Le travail a notamment consisté à préparer les données d'entrée, configurer le pipeline, organiser son exécution sur un environnement HPC avec SLURM et analyser les fichiers de résultats générés.

Le projet a également nécessité **l'identification et la résolution de problèmes techniques liés aux formats des fichiers et à la configuration du pipeline**, jusqu'à son exécution réussie.

## Données utilisées

- **Organisme :** *Gadus morhua* (cabillaud de l'Atlantique)
- **Source :** NCBI Sequence Read Archive (SRA)
- **Échantillons :** SRR2045415 et SRR2045416
- **Type de données :** RNA-seq paired-end (FASTQ)
- **Génome de référence :** gadMor3.0 (GCF_902167405.1)
- **Annotation génomique :** GTF

## Outils et environnement technique

| Outil / technologie | Utilisation dans le projet |
|---|---|
| **Linux / Bash** | Préparation des données et exécution des commandes |
| **SRA Toolkit** | Récupération des données RNA-seq avec `fasterq-dump` |
| **SSH / SCP** | Connexion au cluster et transfert des fichiers |
| **Genotoul / SLURM** | Environnement HPC et soumission des tâches |
| **Nextflow** | Orchestration et exécution du workflow bioinformatique |
| **nf-core/rnavar** | Pipeline de détection de variants à partir de données RNA-seq |
| **STAR** | Alignement des lectures sur le génome de référence |
| **GATK** | Appel de variants génétiques |
| **Samtools** | Production de statistiques d'alignement |
| **FastQC / MultiQC** | Contrôle qualité et synthèse des résultats |

Les outils STAR, GATK, Samtools, FastQC et MultiQC interviennent dans les différentes étapes du pipeline nf-core/rnavar.

## Méthodologie

### 1. Récupération et préparation des données

- Téléchargement de deux échantillons RNA-seq publics depuis NCBI SRA.
- Conversion des données en fichiers FASTQ paired-end à l'aide de `fasterq-dump`.
- Récupération du génome de référence *Gadus morhua* et de son annotation GTF.
- Organisation et compression des fichiers de séquençage.
- Transfert des données sur le cluster Genotoul via SCP.
- Création du fichier `samplesheet.csv` pour renseigner les échantillons à analyser.

### 2. Configuration et exécution du pipeline

- Préparation de l'environnement d'exécution sur Genotoul.
- Configuration du pipeline **nf-core/rnavar** avec Nextflow.
- Définition des chemins d'accès aux données, au génome de référence et à l'annotation.
- Préparation d'un script de soumission **SLURM**.
- Utilisation du profil `genotoul` pour l'exécution sur le cluster HPC.
- Adaptation des paramètres du pipeline aux données disponibles, notamment avec l'option `--skip_baserecalibration`.
- Lancement du workflow et vérification de sa bonne exécution.

Nextflow a permis d'orchestrer les différentes étapes du traitement, notamment le contrôle qualité, l'alignement des lectures et l'appel de variants.

### 3. Exploration et analyse des résultats

- Vérification de la bonne exécution du pipeline.
- Exploration des répertoires générés dans `results_rnavar/`.
- Consultation des statistiques d'alignement produites par Samtools.
- Examen des fichiers VCF bruts et filtrés pour chaque échantillon.
- Identification des types de variants détectés : SNPs et indels.
- Consultation du rapport MultiQC pour examiner les indicateurs de qualité du traitement.

## Résultats obtenus

Le pipeline **nf-core/rnavar a été exécuté avec succès** sur les deux échantillons RNA-seq de *Gadus morhua*.

### Organisation des résultats

L'exploration du répertoire `results_rnavar/` a permis d'identifier les principales sorties du pipeline :

| Dossier | Contenu |
|---|---|
| `genome/` | Fichiers préparés à partir du génome de référence, notamment les index nécessaires à l'alignement |
| `pipeline_info/` | Informations sur l'exécution du pipeline et sa configuration |
| `preprocessing/` | Résultats du prétraitement et du contrôle qualité des données |
| `reports/` | Rapports de synthèse, notamment MultiQC |
| `samtools/` | Statistiques relatives aux alignements |
| `variant_calling/` | Fichiers VCF bruts et filtrés issus de l'appel de variants |

### Analyse des résultats

**Qualité de l'alignement**

L'examen des statistiques `samtools flagstat` a montré un **taux de lectures alignées proche de 100 %** pour les deux échantillons (SRR2045415 et SRR2045416).

**Détection des variants**

Le pipeline a généré des **fichiers VCF bruts et filtrés** pour chaque échantillon. Leur examen a permis d'identifier des variants génétiques de type **SNPs et indels**.

**Rapport MultiQC**

Le fichier `multiqc_report.html`, disponible dans le dossier `reports/`, regroupe les principaux indicateurs de qualité et les statistiques produites au cours du traitement.

Il permet notamment de consulter les informations relatives à la qualité des lectures, aux alignements et aux étapes d'appel de variants.

### Bilan

Ce projet a permis de mettre en œuvre avec succès un workflow d'analyse RNA-seq et de détection de variants génétiques sur un environnement HPC.

Il a également permis de se familiariser avec **l'organisation des sorties d'un pipeline nf-core**, l'examen des statistiques d'alignement et l'exploitation des fichiers VCF et des rapports de qualité.

## Rapport du projet

Le **rapport complet** retrace de manière détaillée l'ensemble du travail réalisé, depuis la préparation des données jusqu'à l'analyse des résultats.

Il documente également les **difficultés techniques rencontrées au cours de l'exécution, les ajustements apportés pour les résoudre et les étapes ayant conduit à la réussite du pipeline**.

Il présente notamment :

- La récupération des données RNA-seq et des fichiers de référence.
- Les commandes Linux utilisées pour préparer et organiser les données.
- Le transfert des fichiers et la préparation de l'environnement Genotoul.
- La configuration de Nextflow et du pipeline nf-core/rnavar.
- Les scripts de soumission SLURM et les paramètres d'exécution.
- Les problèmes techniques rencontrés et les corrections apportées.
- Le déroulement de l'analyse sur le cluster HPC.
- Les résultats obtenus : statistiques d'alignement, fichiers VCF et rapport MultiQC.

📄 **[Consulter le rapport complet du projet](Rapport_NGS_VariantCalling_Nextflow.pdf)**

