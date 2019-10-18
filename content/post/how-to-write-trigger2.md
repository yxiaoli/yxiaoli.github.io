+++
title =  "Build blob trigger and storage queue for microservice"
date =  2019-05-07T13:26:32+02:00
tags = ["Azure","Storage Queue","msg queue","microservice"]
categories = ["tech"]
description = "This article will introduce Azure Event Grid and respective componentsd"
# 此处可以写摘要！
disable_profile = true
menu = ""
banner = "banners/microsoft-azure.png"
+++


In my last blog([here](https://tintinsnowy.com/2019/05/07/how-to-setup-storage-account-trigger-with-event-grid-1/)), I have demonstrated how to set up a Blob trigger uing the Microsoft Azure native
product: **Azure function** to handle the events/updates.

but instead of creating an **Azure funtion** in cloud, we want an on-premise application, or a
microservice with the tigger function embeded.
    
# Solution 2: using RESTful API conncets Azure Storage Account.

## Architecture:
![service architecture](/architecture.jpg)


## Steps:

0. prerequisites
   create **resouce group** -> **Storage account**

1. create Storage queue for storing the messages(events)
  
2. set up Event Grid Subcription
   **Event Subscription** is kind of an agent which wires our Storage Account(which is exactaly our monitoring object)
	wire the Event subscription with specific **Storage Blob**
   ![config the event subscription](https://app.yinxiang.com/shard/s33/res/f11c3b54-57fd-435b-b3f4-d2fc4a934915/Inkedevent%20grid_LI.jpg)

        the endpoit decides the outgoing of our message.
   ![choose endpoint](https://app.yinxiang.com/shard/s33/res/446eab4d-b31e-41f0-88d4-aaa1cca18022/Screenshot%20from%202019-05-22%2016-17-49.png)

3. generate the events
   for example: upload an object

4. fetch the message from Storage Queue

   using the *Azure Storage Queue SDK* to fetch the event message, see below
   
5. Read the payload and trigger your service


```
	import azure.functions as func
	from azure.storage.blob import BlockBlobService
	from azure.storage.queue import (
	    QueueService,
	    QueuePermissions)
	from azure.storage.queue.models import QueueMessageFormat
	import os

	queueName = 'my-message-queue'
	accountName = 'my-storage-account'
	accessKey = 'xxxxxxxxx'
	queue_service = QueueService(account_name=storage_account,
		                 account_key=storage_access_key,
		                 protocol='https')
	queue_service.encode_function = QueueMessageFormat.text_base64encode
	queue_service.decode_function = QueueMessageFormat.text_base64decode
	msgs = queue_service.get_messages(queue_name, num_messages=1, visibility_timeout=5)
	queue_service.delete_messages(queue_name, msgs[0].id, msgs[0].pop_receipt)

```













