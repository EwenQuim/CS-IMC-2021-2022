# Travaux Pratiques Pipeline et Dataflow
Dans ce TP, nous allons implémenter les principe *bronze, silver, gold* dans le cadre de la fusion de données sur l'usage des vélos à Paris. Nous allons utiliser [Azure Data Factory](https://docs.microsoft.com/fr-fr/azure/data-factory/introduction) pour:

1. Configurer Azure Data Factory
2. Importer (copier) des données au format CSV depuis un espace de stockage [AWS S3 Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html) vers un stockage [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/#documentation)
3. Créer un data flow et référencer les deux jeux de données
4. Produire des données *silver* qui fusionnent les deux jeux de données
5. Produire des données *gold* qui agrègent les données créées dans la zone *silver*

## Création et configuration d'Azure Data Factory
1. Créez une instance Azure Data Factory via le portail Azure
2. Connectez-vous au studio associé à cette instance (lien dans l'onglet *Overview*, section *Getting started*). Si nécessaire, passez le studio en anglais (icône de l'engrenage en haut à droite)
3. A gauche, dans la section *Manage* créez deux *Linked services*
    - Choisissez *Amazon S3* et entrez les informations de connexion fournies en cours. Cliquez sur *Test connection* pour vous assurer que la connexion fonctionne.
    - Choisissez *Azure Blob Storage*, puis sélectionnez la souscription *Azure Internal - CentraleSupelec* et le compte de stockage *veloparisazure*. Cliquez sur *Test connection* pour vous assurer que la connexion fonctionne.

## Importation des données depuis AWS S3
Maintenant que vous pouvez vous connecter aux espaces de stockage, copions le fichier *comptage-velo-donnees-compteurs.csv* qui contient les données comptages de S3 avec le Blob Storage.
1. Cliquez sur *Author* à gauche, puis créez un nouveau *Pipeline*
2. Dans la liste des *Activities*, dépliez *Move & transform* et glissez-déposez l'activité *Copy data* sur l'espace de travail juste à droite
3. Cliquez sur l'activité, le panneau du bas vous permet de la configurer.
4. Dans *Source* cliquez sur *New* et allez chercher le fichier *comptage-velo-donnees-compteurs.csv* en passant par le *Linked service* adéquat. Utilisez le bouton *Browse* (icône de dossier) pour vous y aider.
5. Dans *Sink* (la destination de la copie), créez un nouveau dataset dans le *Linked Service* blob storage. Ce fichier sera également au format CSV.
    - Dans *File path* mettez: **aws-s3/import/import-comptage-velo-donnees-compteurs.csv**
    - Cochez *First row as header* (car la première ligne du fichier est l'entête)
6. 
