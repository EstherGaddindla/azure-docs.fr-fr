| Ressource | Gratuit | Partagé (version préliminaire) | De base | Standard | Premium (version préliminaire)</th> |
| --- | --- | --- | --- | --- | --- |
| [Applications Web, mobiles ou API](https://azure.microsoft.com/services/app-service/) par [plan App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Illimité<sup>2</sup> |Illimité<sup>2</sup> |Illimité<sup>2</sup> |
| [Applications logiques](https://azure.microsoft.com/services/app-service/logic/) par [plan App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 par cœur |20 par cœur |
| [Plan App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 par région |10 par groupe de ressources |100 par groupe de ressources |100 par groupe de ressources |100 par groupe de ressources |
| types d'instance de calcul |Partagé |Partagé |Dédié<sup>3</sup> |Dédié<sup>3</sup> |Dédié<sup>3</sup></p> |
| [Montée en charge](../articles/app-service-web/web-sites-scale.md) (nombre maximal d'instances) |1 partagée |1 partagée |3 dédiées<sup>3</sup> |10 dédiées<sup>3</sup> |20 dédiées (50 dans ASE)<sup>3,4</sup> |
| Stockage<sup>5</sup> |1 Go<sup>5</sup> |1 Go<sup>5</sup> |10 Go<sup>5</sup> |50 Go<sup>5</sup> |500 Go<sup>4,5</sup></p> |
| Temps processeur (5 min)<sup>6</sup> |3 minutes |3 minutes |Illimité, facturation aux [tarifs standard](https://azure.microsoft.com/pricing/details/app-service/)</a> |Illimité, facturation aux tarifs standard |Illimité, facturation aux tarifs standard |
| Temps processeur (jour)<sup>6</sup> |60 minutes |240 minutes |Illimité, facturation aux [tarifs standard](https://azure.microsoft.com/pricing/details/app-service/)</a> |Illimité, facturation aux tarifs standard |Illimité, facturation aux tarifs standard |
| Mémoire (1 heure) |1 024 Mo par plan de service d’application |1 024 Mo par application |N/A |N/A |N/A |
| Bande passante |165 Mo |Illimitée, application du [taux de transfert de données](https://azure.microsoft.com/pricing/details/data-transfers/) |Illimitée, application du taux de transfert de données |Illimitée, application du taux de transfert de données |Illimitée, application du taux de transfert de données |
| Architecture de l'application |32 bits |32 bits |32 bits/64 bits |32 bits/64 bits |32 bits/64 bits |
| Web Sockets par instance<sup>7</sup> |5 |35 |350 |Illimité |Illimité |
| [Connexions simultanées du débogueur](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) par application |1 |1 |1 |5 |5 |
| [Sous-domaine azurewebsites.net avec FTP/S et SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md) |X |X |X |X |X |
| [domaines personnalisés](../articles/app-service-web/web-sites-custom-domain-name.md)  | |X |X |X |X |
| domaines personnalisés [Prise en charge SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md) | | |Illimité |Illimité, 5 connexions SSL SNI et 1 connexion SSL IP incluses |Illimité, 5 connexions SSL SNI et 1 connexion SSL IP incluses |
| Équilibrage de charge intégré | |X |X |X |X |
| [Toujours actif](../articles/app-service-web/web-sites-configure.md) | | |X |X |X |
| [Sauvegardes planifiées](../articles/app-service-web/web-sites-backup.md) | | | |Une fois par jour |Toutes les 5 minutes<sup>8</sup> |
| [Mise à l'échelle automatique](../articles/app-service-web/web-sites-scale.md) | | |X |X |X |
| [WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md)<sup>9</sup> |X |X |X |X |X |
| [Azure Scheduler](https://azure.microsoft.com/services/scheduler/)  | |X |X |X |X |
| [Surveillance de point de terminaison](../articles/app-service-web/web-sites-monitor.md) | | |X |X |X |
| [Emplacements intermédiaires](../articles/app-service-web/web-sites-staged-publishing.md) | | | |5 |20 |
| Domaines personnalisés par application</a> | |500 |500 |500 |500 |
| Contrat SLA | |<p> |99,9 % |99,95 %<sup>10</sup> |99,95 %<sup>10</sup> |

<sup>1</sup>Des quotas d'applications et de stockage s'appliquent pour chaque plan App Service, sauf mention contraire.  
<sup>2</sup>Le nombre d'applications qui peuvent être hébergées sur ces ordinateurs dépend de l'activité des applications, de la taille des instances des ordinateurs et de l'utilisation de ressources correspondante.  
<sup>3</sup>Les instances dédiées peuvent être de différentes tailles. Pour plus d'informations, consultez la rubrique [App Service Pricing](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup>La version Premium autorise un maximum de 50 instances de calcul (selon la disponibilité) et 500 Go d'espace disque en cas d'utilisation d'environnements App Service, et 20 instances de calcul et 250 Go de stockage dans les autres cas.  
<sup>5</sup>La limite de stockage est la taille totale du contenu entre toutes les applications du même plan de service d’application. D’autres options de stockage sont disponibles dans [l’environnement App Service](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>Ces ressources sont limitées par les ressources physiques sur les instances dédiées (taille de l'instance et nombre d'instances).  
<sup>7</sup>Si vous mettez à l'échelle une application sur deux instances dans la version de base, vous disposez de 350 connexions simultanées pour chacune des deux instances.  
<sup>8</sup>La version Premium permet des intervalles de sauvegarde est de 5 minutes en cas d'utilisation d'environnements App Service et de 50 fois par jour dans les autres cas.  
<sup>9</sup>Exécution d’exécutables et/ou de scripts personnalisés à la demande, de manière programmée ou en continu en tant que tâche en arrière-plan au sein de votre instance App Service. La fonctionnalité AlwaysOn est nécessaire à l'exécution de tâches web en continu. Azure Scheduler (version Gratuite ou Standard) est nécessaire aux tâches web programmées. Aucune limite n’est prédéfinie quant au nombre de tâches web pouvant s’exécuter dans une instance App Service ; par contre, il existe des limites pratiques qui dépendent de ce que le code d’application tente de faire.   
<sup>10</sup>Contrat de niveau de service de 99,95 % pour les déploiements qui utilisent plusieurs instances avec Azure Traffic Manager configuré pour le basculement.  

