



Selon votre environnement et vos choix, le script peut créer toute l’infrastructure de cluster, y compris le réseau virtuel Azure, les comptes de stockage, les services cloud, le contrôleur de domaine, les bases de données SQL locales ou distantes, le nœud principal et les nœuds supplémentaires du cluster. Le script peut également utiliser l’infrastructure Azure préexistante et créer uniquement les nœuds de cluster HPC.

Pour des informations générales sur la planification d’un cluster HPC Pack, consultez [Évaluation du produit et planification](https://technet.microsoft.com/library/jj899596.aspx) et [Mise en route](https://technet.microsoft.com/library/jj899590.aspx) dans la bibliothèque HPC Pack 2012 R2.

## <a name="prerequisites"></a>Composants requis
* **Abonnement Azure** : vous pouvez utiliser un abonnement dans le service Azure Global ou Azure Chine. Vos limites d’abonnement affectent le nombre et le type de nœuds de cluster que vous pouvez déployer. Pour plus d’informations, consultez [Abonnement Azure et limites, quotas et contraintes du service](../articles/azure-subscription-service-limits.md).
* **Ordinateur client Windows avec Azure PowerShell 0.8.10 ou ultérieur installé et configuré** : pour connaître les instructions d’installation et la procédure de connexion à votre abonnement Azure, consultez [Get started with Azure PowerShell (Prise en main d’Azure PowerShell)](/powershell/azureps-cmdlets-docs).
* **Script de déploiement IaaS de HPC Pack** : téléchargez et décompressez la dernière version du script à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=44949). Vérifiez la version du script en exécutant `New-HPCIaaSCluster.ps1 –Version`. Cet article se base sur la version 4.5.2 du script.
* **Fichier de configuration de script** : créez un fichier XML qui sera utilisé par le script pour configurer le cluster HPC. Pour plus d’informations et des exemples, consultez les sections suivantes de cet article et le fichier Manual.rtf qui accompagne le script de déploiement.

## <a name="syntax"></a>Syntaxe
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Exécutez le script en tant qu’administrateur.
> 
> 

### <a name="parameters"></a>parameters
* **ConfigFile** : spécifie le chemin d’accès du fichier de configuration pour décrire le cluster HPC. Découvrez plus d’informations sur le fichier de configuration dans cette rubrique ou dans le fichier Manual.rtf, dans le dossier contenant le script.
* **AdminUserName** : spécifie le nom d’utilisateur. Si la forêt de domaines est créée par le script, il s’agit du nom d’utilisateur d’administrateur local pour toutes les machines virtuelles et du nom d’administrateur de domaine. Si la forêt de domaines existe déjà, cela indique l’utilisateur du domaine comme nom d’utilisateur d’administrateur local pour installer HPC Pack.
* **AdminPassword** : spécifie le mot de passe de l’administrateur. S’il n’a pas été spécifié dans la ligne de commande, le script vous invite à entrer le mot de passe.
* **HPCImageName** (facultatif) : spécifie le nom d’image de la machine virtuelle HPC Pack à utiliser pour le déploiement du cluster HPC. Ce doit être une image HPC Pack fournie par Microsoft à partir d’Azure Marketplace. Si aucune image n’est spécifiée (recommandé en général), le script choisit la dernière [image HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) publiée. La dernière image est basée sur Windows Server 2012 R2 Datacenter avec HPC Pack 2012 R2 Update 3.
  
  > [!NOTE]
  > Le déploiement échoue si vous ne spécifiez pas d’image HPC Pack valide.
  > 
  > 
* **LogFile** (facultatif) : spécifie le chemin du fichier journal de déploiement. S’il n’a pas été spécifié, le script crée un fichier journal dans le répertoire temp de l’ordinateur qui exécute le script.
* **Force** (facultatif) : supprime toutes les invites de confirmation.
* **NoCleanOnFailure** (facultatif) : spécifie que les machines virtuelles Azure qui n’ont pas été correctement déployées ne sont pas supprimées. Supprimez manuellement ces machines virtuelles avant de réexécuter le script pour poursuivre le déploiement, faute de quoi le déploiement risque d’échouer.
* **PSSessionSkipCACheck** (facultatif) : pour chaque service cloud avec des machines virtuelles déployées par ce script, un certificat auto-signé est automatiquement généré par Azure, et toutes les machines virtuelles du service cloud utilisent ce certificat en tant que certificat Windows Remote Management (WinRM) par défaut. Pour déployer des fonctionnalités HPC dans ces machines virtuelles Azure, le script par défaut installe temporairement ces certificats dans le magasin Ordinateur local\\Autorités de certification racines de confiance de l’ordinateur client pour supprimer l’erreur de sécurité d’autorité de certification non approuvée pendant l’exécution du script. Les certificats sont supprimés quand le script se termine. Si ce paramètre est spécifié, les certificats ne sont pas installés sur l’ordinateur client et l’avertissement de sécurité est supprimé.
  
  > [!IMPORTANT]
  > Ce paramètre n’est pas recommandé pour les déploiements de production.
  > 
  > 

### <a name="example"></a>Exemple
L’exemple suivant crée un cluster HPC Pack à l’aide du fichier de configuration *MyConfigFile.xml* et spécifie les informations d’identification de l’administrateur pour l’installation du cluster.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Considérations supplémentaires
* Le script peut éventuellement activer la soumission de travaux via le portail web HPC Pack ou l’API REST du HPC Pack.
* Le script peut éventuellement exécuter des scripts de pré- et post-configuration personnalisés sur le nœud principal si vous souhaitez installer des logiciels supplémentaires ou configurer d’autres paramètres.

## <a name="configuration-file"></a>Fichier de configuration
Le fichier de configuration pour le script de déploiement est un fichier XML. Le fichier de schéma HPCIaaSClusterConfig.xsd se trouve dans le dossier de script de déploiement du HPC Pack IaaS. **IaaSClusterConfig** est l’élément racine du fichier de configuration qui contient les éléments enfants décrits en détail dans le fichier Manual.rtf dans le dossier de script de déploiement.

