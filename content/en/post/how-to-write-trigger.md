+++
title =  "How to setup Storage Account trigger with Event Grid-1"
date =  2019-05-07T13:26:32+02:00
tags = ["Azure","Cloud","msg queue"]
categories = ["tech"]
description = "This article will introduce Azure Event Grid and respective componentsd"
# 此处可以写摘要！
menu = ""
banner = "banners/spring.jpg"
+++



# Descriptions

we want to build an ’app’ which can listen to a specific 'storage account', whenever
a new file has been uploaded(we take this an event), this app will be triggered to do sth
based on the event.

# About Event Grid

## Components 
**Events** - What happened, alerts,facts

**Event sources** - Where the event took place.

**Event generator/publisher** - e.g. Storage blob

**Topics** - One of an endpoint where publishers send events.

**Event subscriptions** - The endpoint or built-in mechanism to route events, sometimes to more than one handler. Subscriptions are also used by handlers to intelligently filter incoming events.

**Event handlers** - The app or service reacting to the event.(Microsoft built-in service)

**Event Grid Subscription** - like an agent, connect the event generators and the end points.

**Event Grid Domain**- An event domain is a management tool for large numbers of Event Grid topics related to the same application

**Event Grid Topics**

### Architecture

![event subscribe](https://cdn-images-1.medium.com/max/1600/1*TmMc5wsTbUi6CkfrK-ojOA.png)


# About Azure Storage Blob
Azure Blob storage is Microsoft's object storage solution for the cloud.
![relationship](https://docs.microsoft.com/en-us/azure/storage/blobs/media/storage-blob-introduction/blob1.png)

ResourceGroup--> Storage Account--> Containers --> Blobs
 
[docmentation](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview)

# Solution 1: build Azure function

## Method 1: using Azure portal(recommended)


[How to create storage blob trigger](https://docs.microsoft.com/de-de/azure/azure-functions/functions-create-storage-blob-triggered-function)

[Azure function bingding event grid](https://docs.microsoft.com/de-de/azure/azure-functions/functions-bindings-event-grid#create-a-subscription)

    
  1. Create an Azure Function app 
    using the Azure portal
  ![from Azure portal to find out](/function-app-create-flow.png)

  2. Expand your function app and click the **+** button next to Functions. If this is the first function in your function app, select **In-portal** then Continue. Otherwise, go to step three.
  ![in-portal](/function-app-quickstart-choose-portal.png)

  3. choose more Template--> Event Grid trigger 

  4. select **Add Event Grid subscription**.  
     
  5. Create Event Subscription
    you will see more details about configuring the Event Subscription

## Method 2: using Azure cli 
 
  **prerequisites:**
  - Install [Azure Core Tools version 2.x](https://docs.microsoft.com/de-de/azure/azure-functions/functions-run-local#v2).
  - Install the Azure CLI 2.0 +.
az login

  1. create a azure function project:
            
      ```
        $func init myApp
        Select a worker runtime: 
        1. dotnet
        2. node
        3. python (preview)
        4. powershell (preview)
        Choose option: 1
        dotnet
        
        Writing /lhome/yxiaoli/Documents/myApp/.vscode/extensions.json
           
      ``` 
      
      Better to chose dotnet...instead of python
       
   2. generate app function:
   
      ```
        $ cd myApp
        $ func new --name EventTrigger
        Select a template: 
        1. QueueTrigger
        2. HttpTrigger
        3. BlobTrigger
        4. TimerTrigger
        5. DurableFunctionsOrchestration
        6. SendGrid
        7. EventHubTrigger
        8. ServiceBusQueueTrigger
        9. ServiceBusTopicTrigger
        10. EventGridTrigger
        11. CosmosDBTrigger
        12. IotHubTrigger
        Choose option: 10
        Function name: EventTrigger
        
        The function "EventTrigger" was created successfully from the "EventGridTrigger" template.

      ```
   3. customize your function
        
        
   4. upload the Function App to Azure:
      
      ``` 
      $func azure functionapp publish ${your-function-name}
      ```
        
  



# Reference
1. using 'azure.storage.blob'
   return **BlobBlockList**  -->  BlobBlock(id, )
   
1. [Event Grid concepts](https://docs.microsoft.com/en-us/azure/event-grid/concepts)
2. [blobtrigger vs Event Grid](https://docs.microsoft.com/de-de/azure/azure-functions/functions-bindings-storage-blob#triggerb)
3. [Storage queue vs. service bus](https://docs.microsoft.com/de-de/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)
4. [what is event hub](https://hackernoon.com/azure-functions-choosing-between-queues-and-event-hubs-dac4157eee1c
5. [comparing messages](https://docs.microsoft.com/bs-latn-ba/azure/event-grid/compare-messaging-services)
6. [Logic apps vs. azure functions apps ](https://docs.microsoft.com/en-us/azure/event-grid/event-handlers)
