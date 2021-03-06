---
title: "Didacticiel sur l’utilisation de Notification Hubs pour envoyer les dernières nouvelles localisées"
description: "Découvrez comment utiliser Azure Notification Hubs pour envoyer des notifications de dernières nouvelles localisées."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e864e832b4c50644bf4062dee29d34ff9fe2774e


---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Utilisation de Notification Hubs pour envoyer les dernières nouvelles localisées
> [!div class="op_single_selector"]
> * [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Vue d'ensemble
Cette rubrique montre comment utiliser la fonctionnalité de **modèle** d’Azure Notification Hubs pour diffuser des notifications relatives aux dernières nouvelles qui ont été localisées par langue et par appareil. Vous devez commencer ce didacticiel avec l'application Windows Store que vous avez créée dans le cadre du didacticiel [Utilisation de Notification Hubs pour envoyer les dernières nouvelles]. Lorsque vous aurez terminé, vous pourrez vous inscrire aux catégories qui vous intéressent, spécifier une langue dans laquelle recevoir les notifications et recevoir uniquement des notifications Push pour les catégories sélectionnées dans cette langue.

Ce scénario comporte deux parties :

* L'application Windows Store permet aux appareils clients de spécifier une langue et de s'abonner à différentes catégories de dernières nouvelles.
* Le serveur principal diffuse les notifications à l’aide des fonctionnalités de **balise** et de **modèle** d’Azure Notification Hubs.

## <a name="prerequisites"></a>Composants requis
Vous devez avoir suivi le didacticiel [Utilisation de Notification Hubs pour envoyer les dernières nouvelles] et avoir le code à disposition, car le présent didacticiel est basé sur ce code.

Vous avez également besoin de Visual Studio 2012 (ou version ultérieure).

## <a name="template-concepts"></a>Concepts de modèle
Dans le didacticiel [Utilisation de Notification Hubs pour envoyer les dernières nouvelles] , vous avez créé une application qui se sert de **balises** pour s'abonner aux notifications relatives à différentes catégories de nouvelles.
Cependant, de nombreuses applications sont destinées à plusieurs marchés et doivent donc être localisées. Cela signifie que le contenu des notifications proprement dites doit lui aussi être localisé et envoyé au bon ensemble d’appareils.
Dans cette rubrique, nous allons vous montrer comment utiliser la fonctionnalité de **modèle** de Notification Hubs pour facilement envoyer des notifications de dernières nouvelles localisées.

Remarque : pour envoyer des notifications localisées, vous pouvez notamment créer plusieurs versions de chaque balise. Par exemple, pour prendre en charge l'anglais, le français et le mandarin, nous aurions besoin de trois balises différentes pour les nouvelles internationales : « world_en », « world_fr » et « world_ch ». Il faudrait ensuite que nous envoyions une version localisée des nouvelles internationales à chacune de ces balises. Dans cette rubrique, nous utilisons des modèles afin d'éviter la prolifération de balises et d'éliminer la nécessité d'envoyer plusieurs messages.

À un haut niveau, les modèles permettent de spécifier comment un appareil particulier reçoit une notification. Le modèle spécifie le format de charge utile exact en se référant aux propriétés qui font partie du message envoyé par le serveur principal de votre application. Aux fins de notre exemple, nous allons envoyer un message de paramètres régionaux contenant toutes les langues prises en charge :

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ensuite, nous allons nous assurer que les appareils s'inscrivent avec un modèle qui se réfère à la bonne propriété. Par exemple, une application Windows Store qui veut recevoir un simple message toast doit s’inscrire pour le modèle suivant, avec les balises correspondantes :

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Les modèles sont une fonctionnalité très puissante sur laquelle vous pouvez obtenir plus d’informations en lisant notre article [Modèles](notification-hubs-templates-cross-platform-push-messages.md) . 

## <a name="the-app-user-interface"></a>Interface utilisateur de l’application
Nous allons maintenant modifier l’application de dernières nouvelles que vous avez créée à la rubrique [Utilisation de Notification Hubs pour envoyer les dernières nouvelles] pour envoyer les dernières nouvelles localisées à l’aide de modèles.

Dans votre application Windows Store :

Modifiez le fichier MainPage.xaml pour qu'il inclue une zone de liste modifiable pour les paramètres régionaux :

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-the-windows-store-client-app"></a>Création de l’application cliente Windows Store
1. Dans la classe Notifications, ajoutez un paramètre régional aux méthodes *StoreCategoriesAndSubscribe* et *SubscribeToCateories*.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    Notez qu’au lieu d’appeler la méthode *RegisterNativeAsync*, nous appelons *RegisterTemplateAsync* : nous inscrivons un format de notification spécifique dans lequel le modèle dépend des paramètres régionaux. Nous avons également fourni un nom pour le modèle (« localizedWNSTemplateExample »), parce qu’il est possible que nous inscrivions plusieurs modèles (par exemple un pour les notifications toast et un pour les vignettes) et nous devons donc les nommer pour pouvoir les mettre à jour ou les supprimer.
   
    Notez que si un appareil inscrit plusieurs modèles avec la même balise, un message entrant ciblant cette balise entraînera l'envoi de plusieurs notifications à l'appareil (un pour chaque modèle). Ce comportement s'avère utile lorsque le même message logique doit générer plusieurs notifications visuelles, par exemple affichant un badge et un toast dans une application Windows Store.
2. Ajoutez la méthode suivante pour extraire les paramètres régionaux stockés :
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. Dans le fichier MainPage.xaml.cs, mettez le gestionnaire de clics de bouton à jour en extrayant la valeur actuelle de la zone de liste déroulante Paramètres régionaux et en la fournissant à l’appel de la classe Notifications, comme indiqué ci-après :
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. Enfin, dans votre fichier App.xaml.cs, veillez à mettre à jour votre méthode `InitNotificationsAsync` pour extraire les paramètres régionaux et les utiliser lors de l’abonnement :
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a>Envoi de notifications localisées à partir de votre serveur principal
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Concepts de modèle]: #concepts
[Interface utilisateur de l’application]: #ui
[Création de l’application cliente Windows Store]: #building-client
[Envoi de notifications à partir de votre serveur principal]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Service mobile]: /develop/mobile/tutorials/get-started
[Notification des utilisateurs avec Notification Hubs : ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notification des utilisateurs avec Notification Hubs : Mobile Services]: /manage/services/notification-hubs/notify-users
[Utilisation de Notification Hubs pour envoyer les dernières nouvelles]: /manage/services/notification-hubs/breaking-news-dotnet

[Soumettre une application]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Mes applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Kit de développement logiciel (SDK) Live pour Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Prise en main de Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Prise en main des données]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Prise en main de l'authentification]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Prise en main des notifications Push]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Notifications Push pour les utilisateurs d’applications]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Autorisation des utilisateurs avec des scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript et HTML]: /develop/mobile/tutorials/get-started-with-push-js

[objet wns]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Recommandations relatives à Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Procédures Notification Hubs pour iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Vue d’ensemble de Notification Hubs pour Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx



<!--HONumber=Nov16_HO3-->


