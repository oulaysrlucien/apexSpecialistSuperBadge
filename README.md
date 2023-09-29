# Apex Specialist

## Ce que vous devez accomplir pour gagner ce superbadge :

1. Automatiser la création d'enregistrements à l'aide de déclencheurs Apex.
2. Synchroniser les données Salesforce avec un système externe en utilisant des appels REST asynchrones.
3. Planifier la synchronisation en utilisant du code Apex.
4. Tester la logique d'automatisation pour confirmer les effets secondaires des déclencheurs Apex.
5. Tester la logique d'intégration en utilisant des simulations d'appels externes (callout mocks).
6. Tester la logique de planification pour confirmer que l'action est mise en file d'attente.


## Concepts testés avec ce Superbadge :

* Déclencheurs Apex.
* Apex asynchrone.
* Intégration Apex.
* Tests Apex.

## Travail préalable et notes :

* Prenez un stylo et du papier. Vous voudrez peut-être prendre des notes pendant que vous lisez les exigences.
* Consultez le document d'aide du défi Trailhead Apex Specialist Superbadge pour obtenir des ressources et de la documentation détaillées.
* Utilisez les conventions de nommage spécifiées dans le document des exigences pour garantir un déploiement réussi.
* Examinez le schéma de données dans votre organisation Salesforce modifiée en lisant les exigences détaillées ci-dessous.


## Configuration de l'org de développement :

1. Créez un nouveau Trailhead Playground ou un Developer Edition Org pour ce superbadge. Utiliser cette org à d'autres fins pourrait poser des problèmes lors de la validation du défi. Si vous choisissez d'utiliser une org de développement, assurez-vous de déployer Mon Domaine (My Domain) pour tous les utilisateurs. Le package que vous installerez contient des composants Lightning personnalisés qui n'apparaissent que lorsque Mon Domaine est déployé.
2. Installez ce [package non verrouillé](https://login.salesforce.com/?ec=302&startURL=%2Fpackaging%2FinstallPackage.apexp%3Fp0%3D04t6g000008av9iAAA) (ID du package : 04t6g000008av9iAAA). Ce package contient des métadonnées que vous utiliserez pour accomplir ce défi. Si vous avez des difficultés à installer ce package, suivez les étapes de l'article d'aide [Installer un package ou une application pour accomplir un défi Trailhead](https://trailhead.salesforce.com/fr/help?article=Installing-a-package-or-app-to-complete-a-Trailhead-challenge).
3. Ajoutez les valeurs de liste de sélection Repair et Routine Maintenance au champ Type de l'objet Case.
4. Mettez à jour l'attribution de la mise en page de l'objet Cas pour utiliser la mise en page Cas (HowWeRoll) pour votre profil.
5. Renommez l'onglet/étiquette de l'objet Cas en Maintenance Request.
6. Mettez à jour l'attribution de la mise en page de l'objet Produit pour utiliser la mise en page Produit (HowWeRoll) pour votre profil.
7. Renommez l'onglet/étiquette de l'objet Product en Equipment.
8. Utilisez App Launcher pour accéder à l'onglet Create Default Data de l'application How We Roll Maintenance. Cliquez sur Create Data pour générer des données d'exemple pour l'application.
9. Examinez les enregistrements nouvellement créés pour vous familiariser avec le modèle de données.

### Notes 


## Use Case 
Il existe deux types de personnes dans ce monde : ceux qui voyageraient en véhicule récréatif (VR) et ceux qui ne le feraient pas. La communauté des VR augmente de manière exponentielle dans le monde entier. Au cours des dernières années, HowWeRoll Rentals, la plus grande entreprise de location de VR au monde, a multiplié par dix sa présence mondiale et sa flotte de camping-cars. HowWeRoll propose aux voyageurs des services de location de VR de qualité supérieure et une assistance routière. Leur slogan est : "Nous offrons un excellent service, car c'est ainsi que nous fonctionnons !" Leur flotte de location comprend tous les styles de véhicules de camping-car, des maisons roulantes extra-larges et luxueuses aux rétro Winnebagos minimalistes.

Vous avez été embauché en tant que principal développeur Salesforce pour automatiser et étendre la portée de HowWeRoll. Pour les voyageurs, tous les voyages ne se déroulent pas comme prévu. Heureusement, HowWeRoll dispose d'une équipe de réparation de VR exceptionnelle capable d'intervenir pour toute demande d'entretien, où que vous soyez. Ces réparations traitent divers problèmes techniques, de l'essieu cassé à la fosse septique bouchée.

À mesure que l'entreprise se développe, la flotte de location de HowWeRoll se développe également. Bien que cela soit bénéfique pour les affaires, le processus actuel de service et d'entretien est difficile à mettre à l'échelle. En plus des demandes de service pour des équipements cassés ou défectueux, les demandes d'entretien régulier des véhicules ont augmenté de manière exponentielle. Sans des vérifications d'entretien régulières, la flotte de location est exposée à des pannes évitables.

C'est là que vous intervenez ! HowWeRoll a besoin que vous automatisiez leur système d'entretien régulier basé sur Salesforce. Vous veillerez à ce que tout ce qui pourrait causer des dommages inutiles au véhicule, voire mettre en danger le client, soit signalé. Vous intégrerez également Salesforce avec le système back-office de HowWeRoll qui gère l'inventaire de l'entrepôt. Ce système complètement séparé doit se synchroniser régulièrement avec Salesforce. La synchronisation garantit que le siège de HowWeRoll (HQ) sait exactement combien d'équipements sont disponibles lorsqu'une demande d'entretien est faite, et les alerte lorsqu'ils ont besoin de commander plus d'équipements.


## Objets standard :

Vous travaillerez avec les objets standard suivants :

* Maintenance Request (renommée Case) - Demandes de service pour des véhicules endommagés, des dysfonctionnements et de l'entretien régulier.
* Equipment (renommé Produit) - Pièces et articles stockés dans l'entrepôt et utilisés pour réparer ou entretenir les camping-cars.

## Objets personnalisés :

* Vehicle - Véhicules de la flotte de location de HowWeRoll.
* Equipment Maintenance Item - Associe un enregistrement d'Équipement à un enregistrement de Demande d'entretien, indiquant l'équipement nécessaire pour la demande d'entretien.



## Exigences métier / Business Requirements :
Cette section représente le résultat de vos réunions avec les parties prenantes clés de HowWeRoll. Il s'agit de votre plan pour automatiser de manière programmatique le support et la maintenance de leur activité.

## Automatiser les Demandes d'Entretien
Avec la croissance exponentielle de la popularité des VR dans le monde entier, HowWeRoll fournit des centaines de véhicules de luxe et économiques supplémentaires à travers le monde. Cette augmentation de leur stock de location s'accompagne inévitablement d'une augmentation des pannes d'équipement. HowWeRoll dispose déjà d'un processus pour gérer ces pannes, mais ils souhaitent que vous construisiez une automatisation pour leur entretien régulier. Vous créerez un processus programmatique qui planifie automatiquement des contrôles réguliers de l'équipement en fonction de la date d'installation de l'équipement.


Lorsqu'une demande d'entretien existante de type Réparation ou Entretien régulier est clôturée, créez une nouvelle demande d'entretien pour un contrôle régulier futur. Cette nouvelle demande d'entretien est liée aux mêmes enregistrements de Véhicule et d'Équipement que la demande initialement clôturée. Pour des raisons de suivi des enregistrements, les éléments d'entretien de l'équipement existants doivent rester liés à la demande de clôture initiale, de sorte que de nouveaux enregistrements doivent être créés. Le type de cette nouvelle demande doit être défini comme Entretien régulier. Le champ Sujet ne doit pas être nul et le champ Date de rapport reflète le jour où la demande a été créée. N'oubliez pas que tous les équipements ont des cycles d'entretien.

Calculez les dates d'échéance des demandes d'entretien en utilisant le cycle d'entretien défini dans les enregistrements d'équipement associés. Si plusieurs équipements sont utilisés dans la demande d'entretien, définissez la date d'échéance en appliquant le cycle d'entretien le plus court à la date actuelle.

Concevez le code pour qu'il fonctionne aussi bien avec des demandes d'entretien individuelles que groupées. Rendez le système capable de traiter avec succès environ 300 enregistrements de demandes d'entretien hors ligne qui sont programmés pour être importés ensemble. Pour l'instant, ne vous inquiétez pas des modifications qui interviennent sur l'enregistrement de l'équipement lui-même.

Exposez également la logique pour d'autres utilisations dans l'organisation. Séparez le déclencheur (nommé MaintenanceRequest) de la logique de l'application dans le gestionnaire (nommé MaintenanceRequestHelper). Cette configuration facilite la délégation des actions et l'extension de l'application à l'avenir.

## Synchroniser la Gestion des Stocks

En plus de l'entretien de l'équipement, concevez la synchronisation des données de gestion des stocks de HowWeRoll avec le système externe de l'entrepôt d'équipement. Bien que l'ensemble du siège social utilise Salesforce, l'équipe de l'entrepôt travaille toujours sur un système hérité distinct. Avec cette intégration, l'inventaire dans Salesforce est mis à jour après que l'équipement est pris dans l'entrepôt pour effectuer l'entretien d'un véhicule.

Écrivez une classe qui effectue un appel REST vers un système d'entrepôt externe pour obtenir une liste d'équipements à mettre à jour. La réponse JSON de l'appel renvoie les enregistrements d'équipement que vous insérez dans Salesforce. Au-delà de l'inventaire, assurez-vous que d'autres modifications potentielles de l'entrepôt sont également transférées dans Salesforce. Votre classe mappe les champs suivants :

* Replacement part (always true)
* Cost
* Current inventory
* Lifespan
* Maintenance cycle
* Warehouse SKU

Utilisez le warehouse SKU comme ID externe pour identifier les enregistrements d'équipement à mettre à jour dans Salesforce.

Bien que HowWeRoll soit une entreprise internationale, les bureaux distants suivent l'horaire de travail du siège social. Par conséquent, toutes les demandes d'entretien sont traitées pendant les heures normales de travail du siège social. Vous devez mettre à jour les données Salesforce en dehors des heures de travail (à 1h00 du matin). Cette logique s'exécute quotidiennement afin que l'inventaire soit à jour chaque matin au siège social.

## Créer des Tests Unitaires
Testez votre code pour vous assurer qu'il s'exécute correctement avant de le déployer en production.

Tout d'abord, testez le déclencheur pour vous assurer qu'il fonctionne comme prévu. Suivez les meilleures pratiques en testant à la fois les cas d'utilisation positifs (lorsque le déclencheur doit se déclencher) et les cas d'utilisation négatifs (lorsque le déclencheur ne doit pas se déclencher). Bien sûr, réussir un test ne signifie pas nécessairement que vous avez tout correctement fait. Ajoutez donc des assertions dans votre code pour vous assurer de ne pas obtenir de faux positifs. Pour votre test positif, assurez-vous que tout a été créé correctement, y compris les relations avec le véhicule et l'équipement, ainsi que la date d'échéance. Pour votre test négatif, assurez-vous qu'aucun ordre de travail n'a été créé.

Comme mentionné précédemment, la vague importante de demandes d'entretien pourrait potentiellement être chargée en une fois. Le nombre sera probablement d'environ 200, mais pour tenir compte des pics éventuels, prévoyez que votre classe puisse traiter avec succès au moins 300 enregistrements. Pour tester cela, incluez un cas d'utilisation positif pour 300 demandes d'entretien et assurez-vous que votre test s'est déroulé comme prévu.

Lorsque vous avez une couverture de code à 100 % sur votre déclencheur et votre gestionnaire, écrivez des cas de test pour vos classes d'appelout et de planification Apex. Vous devez avoir une couverture de code à 100 % pour tout le code Apex dans votre organisation.

Assurez-vous que votre code fonctionne comme prévu dans le contexte de planification en vérifiant qu'il s'exécute après Test.stopTest() sans exception. Vérifiez également qu'un travail asynchrone planifié est dans la file d'attente. Les classes de test pour le service d'appelout et le test planifié doivent également avoir une couverture de test à 100 %.


----------------------------------------------------------------------------------------------------


# Salesforce DX Project: Next Steps

Now that you’ve created a Salesforce DX project, what’s next? Here are some documentation resources to get you started.

## How Do You Plan to Deploy Your Changes?

Do you want to deploy a set of changes, or create a self-contained application? Choose a [development model](https://developer.salesforce.com/tools/vscode/en/user-guide/development-models).

## Configure Your Salesforce DX Project

The `sfdx-project.json` file contains useful configuration information for your project. See [Salesforce DX Project Configuration](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) in the _Salesforce DX Developer Guide_ for details about this file.

## Read All About It

- [Salesforce Extensions Documentation](https://developer.salesforce.com/tools/vscode/)
- [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
- [Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)
