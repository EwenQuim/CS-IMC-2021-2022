# Travaux Pratiques Hot Pizzas
My customers deserve hot pizzas!!!  
<br />
![image](https://user-images.githubusercontent.com/20154628/145547104-43a2d9b7-1754-409b-8bbf-2060ee0f127e.png)  
<br />
![image](https://user-images.githubusercontent.com/20154628/145550769-ad5c56e9-bbc5-459f-9ed4-d00260ec4125.png)  
<br />
![image](https://user-images.githubusercontent.com/20154628/145551450-e5af6b3d-9412-407b-827c-3129783dbded.png)
<br />
![image](https://user-images.githubusercontent.com/20154628/145551474-327f4179-658d-4638-aac8-5b24d1415b0f.png)
<br />
  
## Création de l'Injecteur d'évènements

Créer un service Azure Event Hub : https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create  
<br />
Créer une Azure Logic Apps pour injecter des évènements dans l'Event Hub : https://docs.microsoft.com/en-us/azure/logic-apps/quickstart-create-first-logic-app-workflow  
<br />
Exemple d'évènement au format JSON:  
```
{  
     "DeviceId": "dev01",  
     "Speed": 40,  
     "Temperature": 70  
}
```
<br />
Commencer le workflow Logic Apps avec un Step de type "Recurrence" (définir la fréquence)
<br />
Utiliser un Step de type "Send Event"
<br />
Utiliser la fonction random (rand) dans le JSON pour faire varier les valeurs...
<br /><br />
On doit arriver à quelque chose comme ça :
<br />
https://user-images.githubusercontent.com/20154628/145559457-300e909f-ac61-47af-9f91-a866d3191426.png 
<br />

## Création du Consommateur d'évènements

Créer une autre Azure Logic Apps
<br />
Commencer le workflow par un Step de type trigger "When events are available in Event Hub"
<br /><br />
On doit arriver à quelque chose comme ça :
<br />
https://user-images.githubusercontent.com/20154628/145561491-daebde95-945d-4476-8139-d76c4f36e399.png
<br />


