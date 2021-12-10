# Travaux Pratiques Hot Pizzas
My customers deserve hot pizzas!!!

![image](https://user-images.githubusercontent.com/20154628/145547104-43a2d9b7-1754-409b-8bbf-2060ee0f127e.png)
![image](https://user-images.githubusercontent.com/20154628/145546851-d89a1c02-4353-4cc2-b7b4-ab10b3818e6b.png)
  
## Création de l'Injecteur d'évènements

Créer un service Azure Event Hub : https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create
Créer une Azure Logic Apps pour injecter des évènements dans l'Event Hub : https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-first-logic-app-workflow

Exemple d'évènement au format JSON:
{
     "DeviceId": "dev01",
     "Speed": 40,
     "Temperature": 70
}

## Création du Consommateur d'évènements

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal 

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal 

