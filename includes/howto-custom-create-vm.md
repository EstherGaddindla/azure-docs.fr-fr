# <a name="how-to-create-a-custom-virtual-machine"></a>Création d'une machine virtuelle personnalisée
Une machine virtuelle *personnalisée* fait référence à une machine virtuelle créée via la méthode **À partir de la galerie**, qui vous permet de configurer plus d’options que la méthode **Création rapide**. Ces options sont les suivantes :

* Davantage de choix concernant l'image à utiliser pour créer la machine virtuelle
* Connexion de la machine virtuelle à un réseau virtuel
* Ajout de la machine virtuelle à un service cloud existant
* Ajout de la MV à un groupe à haute disponibilité

> [!IMPORTANT]
> si vous souhaitez que la machine virtuelle utilise un réseau virtuel pour pouvoir vous y connecter directement par nom d’hôte ou configurer des connexions entre différents locaux, veillez à spécifier le réseau virtuel lors de la création de la machine virtuelle. Vous pouvez configurer une machine virtuelle pour qu'elle rejoigne uniquement un réseau virtuel lorsque vous la créez. Pour plus d’informations sur les réseaux virtuels, consultez la page [Présentation du réseau virtuel Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

1. Connectez-vous au [portail Azure](http://manage.windowsazure.com).
2. Dans la barre de commandes, cliquez sur **New**.
3. Cliquez sur **Compute**, sur **Virtual Machine**, puis sur **From Gallery**.
4. Sélectionnez l'image que vous souhaitez utiliser, puis cliquez sur la flèche pour continuer.
5. Si plusieurs versions de l'image sont disponibles, dans **Version Release Date**, choisissez la version à utiliser.
6. Dans **Virtual Machine Name**, tapez le nom que vous souhaitez utiliser pour la machine virtuelle.
7. Utilisez **Tier** et **Size** pour sélectionner une taille adéquate pour la machine virtuelle. La configuration maximale de la machine virtuelle, ainsi que son prix, varieront en fonction de la taille que vous sélectionnerez. Pour obtenir des informations de configuration détaillées, consultez la page [Tailles de machines virtuelles et services cloud pour Microsoft Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).
8. Dans **New User Name**, tapez le nom du compte administratif que vous voulez utiliser pour gérer le serveur.
9. Dans **New Password**, tapez un mot de passe fort pour le compte administratif. Dans **Confirm Password**, entrez de nouveau le mot de passe.
10. Cliquez sur la flèche pour continuer.
11. Dans **Cloud Service**, effectuez l'une des actions suivantes :
    
    * S'il s'agit de la première ou de la seule machine virtuelle du service cloud, sélectionnez **Create a New Cloud Service**. Dans **Cloud Service DNS Name**, tapez un nom comprenant entre 3 et 24 caractères (minuscules et chiffres uniquement). Ce nom fait alors partie de l’URI servant à contacter la machine virtuelle via le service cloud.
    * Si cette machine virtuelle est ajoutée à un service cloud, sélectionnez-la dans la liste.
    
    > [!NOTE]
    > Pour plus d’informations sur le placement des machines virtuelles dans le même service cloud, consultez la rubrique [Connexion de machines virtuelles dans un service cloud](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).
    > 
    > 
12. Dans **Region/Affinity Group/Virtual Network**, sélectionnez la région, le groupe d'affinités ou le réseau virtuel à utiliser pour la machine virtuelle. Pour en savoir plus sur les groupes d’affinités, voir [À propos des groupes d’affinités de réseau virtuel](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md).
13. Dans **Storage Account**, sélectionnez un compte de stockage existant pour le fichier de disque dur virtuel ou utilisez un compte de stockage automatiquement généré. Un seul compte de stockage est automatiquement créé par région. Toutes les autres machines virtuelles créées avec ce paramètre sont placées dans ce compte de stockage. Vous êtes limité à 20 comptes de stockage.
14. Si vous souhaitez que la machine virtuelle fasse partie d’un groupe à haute disponibilité, accédez à **Availability Set**, sélectionnez **Create availability set** ou ajoutez-le à un groupe à haute disponibilité existant.
    
    **Remarque**: les machines virtuelles d’un groupe à haute disponibilité sont déployées vers différents domaines d’erreur. Le placement de plusieurs machines virtuelles dans un groupe à haute disponibilité permet de vous assurer de la disponibilité de votre application lors de défaillances réseau, de défaillances matérielles d'un disque dur local et de temps d'arrêt planifiés.
15. Sous **Endpoints**, passez en revue les nouveaux points de terminaison qui seront créés pour autoriser les connexions à la machine virtuelle, tels que le Bureau à distance ou un client Secure Shell (SSH), par exemple. Vous pouvez également ajouter des points de terminaison maintenant ou les créer ultérieurement. Pour obtenir des instructions sur leur création à une date ultérieure, consultez la page [Configuration des points de terminaison sur une machine virtuelle](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
16. Sous **VM Agent**, indiquez si vous souhaitez installer l'agent MV. Cet agent fournit l'environnement dans lequel vous installez les extensions qui permettent d'interagir avec la machine virtuelle. Pour plus d'informations, consultez la page [Gestion des extensions](http://go.microsoft.com/FWLink/p/?LinkID=390493).
17. Cliquez sur la flèche pour créer la machine virtuelle.
    
    ![Création d’une machine virtuelle personnalisée réussie](./media/howto-custom-create-vm/VMSuccessWindows.png)

## <a name="next-steps"></a>Étapes suivantes
Une fois créée, la machine virtuelle démarre automatiquement. Lorsque le portail indique qu'elle est active, vous pouvez vous connecter à la machine virtuelle. Pour plus d'informations, consultez l'un des articles suivants :

* [Connexion à une machine virtuelle exécutant Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Connexion à une machine virtuelle exécutant Windows Server](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

