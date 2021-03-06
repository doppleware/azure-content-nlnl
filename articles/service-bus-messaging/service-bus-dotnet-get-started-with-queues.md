<properties
    pageTitle="Aan de slag met Service Bus-wachtrijen | Microsoft Azure"
    description="Een C#-consoletoepassing schrijven voor Service Bus-berichten"
    services="service-bus-messaging"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus-messaging"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/23/2016"
    ms.author="jotaub;sethm"/>


# Aan de slag met Service Bus-wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## Wat wordt bereikt

In deze zelfstudie voltooien we het volgende:

1. Een Service Bus-naamruimte maken met de Azure-portal.

2. Een Service Bus-berichtenwachtrij maken met de Azure-portal.

3. Een consoletoepassing schrijven om een bericht te verzenden.

4. Een consoletoepassing schrijven om berichten te ontvangen.

## Vereisten

1. [Visual Studio 2013 of Visual Studio 2015](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2015 gebruikt.

2. Een Azure-abonnement.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 1. Een naamruimte maken met de Azure-portal

Als u al een Service Bus-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een wachtrij maken met Azure Portal](#2-create-a-queue-using-the-azure-portal)

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## 2. Een wachtrij maken met de Azure-portal

Als u al een Service Bus-wachtrij hebt gemaakt, gaat u naar het gedeelte [Berichten naar de wachtrij verzenden](#3-send-messages-to-the-queue).

[AZURE.INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## 3. Berichten naar de wachtrij verzenden

We maken een C#-consoletoepassing met Visual Studio om berichten naar de wachtrij te verzenden.

### Een consoletoepassing maken

1. Visual Studio starten en een nieuwe consoletoepassing maken.

### Het Service Bus NuGet-pakket toevoegen

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.

2. Klik op het tabblad **Bladeren**, zoek naar Microsoft Azure Service Bus en selecteer het item **Microsoft Azure Service Bus**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.

    ![Selecteer een NuGet-pakket][nuget-pkg]

### Schrijf wat code om een bericht naar de wachtrij te verzenden

1. Voeg de volgende instructie boven in het bestand Program.cs toe.

    ```
    using Microsoft.ServiceBus.Messaging;
    ```
    
2. Voeg de volgende code toe aan de methode `Main`, stel de variable **connectionString** in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel **queueName** in als de wachtrijnaam die is gebruikt bij het maken van de wachtrij.

    ```
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```

    Zo zou het bestand Program.cs er moeten uitzien.

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;

    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                client.Send(message);
            }
        }
    }
    ```
  
3. Voer het programma uit en ga naar Azure Portal. Klik op de naam van uw wachtrij in de blade **Overzicht** voor de naamruimte. De waarde voor **Aantal actieve berichten** moet nu 1 zijn.
    
      ![Aantal berichten][queue-message]
    
## 4. Berichten ontvangen uit de wachtrij

1. Maak een nieuwe consoletoepassing en voeg een verwijzing toe naar het Service Bus-pakket NuGet, zoals ook met de voorgaande verzendtoepassing is gedaan.

2. Voeg boven in het bestand Program.cs de volgende `using`-instructie toe.
  
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
  
3. Voeg de volgende code toe aan de methode `Main`, stel de variable **connectionString** in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel **queueName** in als de wachtrijnaam die u hebt gebruikt bij het maken van de wachtrij.

    ```
    var connectionString = "";
    var queueName = "samplequeue";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
  
    Console.ReadLine();
    ```

    Zo zou het bestand Program.cs er moeten uitzien:

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;
  
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
  
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
  
          Console.ReadLine();
        }
      }
    }
    ```
  
4. Voer het programma uit en controleer de portal. Merk op dat de waarde voor **Wachtrijlengte** nu 0 moet zijn.

    ![Wachtrijlengte][queue-message-receive]
  
Gefeliciteerd. U hebt nu een wachtrij gemaakt, een bericht verzonden en een bericht ontvangen.

## Volgende stappen

Bekijk onze [GitHub-opslagplaats met voorbeelden](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) die enkele van de meer geavanceerde functies van Azure Service Bus-berichten laten zien.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->

[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Sep16_HO5-->


