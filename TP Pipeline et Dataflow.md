# Travaux Pratiques Pipeline et Dataflow
Dans ce TP, nous allons implémenter les principe *bronze, silver, gold* dans le cadre de la fusion de données sur l'usage des vélos à Paris. Nous allons utiliser [Azure Data Factory](https://docs.microsoft.com/fr-fr/azure/data-factory/introduction) pour:

1. Configurer Azure Data Factory
2. Importer (copier) des données au format CSV depuis un espace de stockage [AWS S3 Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html) vers un stockage [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/#documentation)
3. Créer un data flow et référencer les deux jeux de données
4. Produire des données *silver* qui fusionnent les deux jeux de données
5. Produire des données *gold* qui agrègent les données créées dans la zone *silver*

Les fichiers que nous utilisons sont:
    - *comptage-velo-donnees-compteurs.csv* qui contient le nombre de passages par station de comptage (environ 1M d'enregistrements), initialement hébergé sur AWS
    - *comptage-velo-compteurs.csv* qui contient les informations des compteurs eux-mêmes (un peu moins de 300 enregistrements), hébergé sur Azure

## Création et configuration d'Azure Data Factory
1. Créez une instance Azure Data Factory via le portail Azure
2. Connectez-vous au studio associé à cette instance (lien dans l'onglet *Overview*, section *Getting started*). Si nécessaire, passez le studio en anglais (icône de l'engrenage en haut à droite)
3. A gauche, dans la section *Manage* créez deux *Linked services*
    - Choisissez *Amazon S3* et entrez les informations de connexion fournies en cours. Cliquez sur *Test connection* pour vous assurer que la connexion fonctionne.
    - Choisissez *Azure Blob Storage*, puis sélectionnez la souscription *Azure Internal - CentraleSupelec* et le compte de stockage *veloparisazure*. Cliquez sur *Test connection* pour vous assurer que la connexion fonctionne.

## Importation des données depuis AWS S3 et création des datasets
Maintenant que vous pouvez vous connecter aux espaces de stockage, copions le fichier *comptage-velo-donnees-compteurs.csv* qui contient les données comptages de S3 avec le Blob Storage.
1. Cliquez sur *Author* à gauche, puis créez un nouveau *Pipeline*
2. Dans la liste des *Activities*, dépliez *Move & transform* et glissez-déposez l'activité *Copy data* sur l'espace de travail juste à droite
3. Cliquez sur l'activité, le panneau du bas vous permet de la configurer.
4. Dans *Source* cliquez sur *New* et allez chercher le fichier *comptage-velo-donnees-compteurs.csv* en passant par le *Linked service* adéquat. Utilisez le bouton *Browse* (icône de dossier) pour vous y aider, nommez le *compteurs-aws*
5. Dans *Sink* (la destination de la copie), créez un nouveau dataset dans le *Linked Service* blob storage. Ce fichier sera également au format CSV.
    - Nommez le dataset *comptages_azure*
    - Dans *File path* mettez: **aws-s3/import-[votrenom]** et rien pour *File*
    - Décochez *First row as header*
    - Dans *Import schema* choisissez *None*
6. Cliquez sur *Validate all* en haut de l'écran, il ne devrait pas y avoir d'erreur. S'il y en a, cliquez sur l'erreur et le portail vous pointera vers cette dernière
7. Cliquez sur *Publish all* pour sauvegarder
8. Lancez votre pipeline en cliquant sur *Add trigger* puis *Trigger now*. Vous pouvez observer votre run dans l'onglet *Monitor*
9. Depuis le portail Azure, allez dans l'espace de stockage *veloparisazure* et vous devriez retrouver votre fichier en utilisant le [Storage browser](https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/43515dcd-cf02-45fd-bc30-f2c80dccc7dc/resourcegroups/datatransformation-rg/providers/Microsoft.Storage/storageAccounts/veloparisazure/storagebrowser)
10. Référençons maintenant les données des compteurs en créant un nouveau dataset pointant vers le fichier **infocompteurs/ /comptage-velo-compteurs.csv** (utilisez le bouton *Browse*) et cochez *First row as header*

## Jointure des données
Nos données sont maintenant réunies dans notre Data Lake (le blob Storage). Effectuons la jointure!
1. Pour les deux datasets dans Azure, *comptages_azure* et *compteurs*:
    - Dans l'onglet Connection, choisissez ";" (Semicolon) comme *Column delimiter*
    - Toujours dans l'onglet Connection, assurez-vous que *First row as header* est bien coché
    - Dans l'onglet Schema, cliquez sur *Import schema* puis *From connection/store*
