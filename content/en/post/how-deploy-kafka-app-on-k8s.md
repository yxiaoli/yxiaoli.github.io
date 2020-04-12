+++
title =  "How to depoly MapR kafka app on kubernetes"
date =  2019-10-08T09:07:55+01:00
tags = ["cloud","kubernetes","INFR"]
categories = ["tech"]
description = "THe How-to guids to set up kafka python applications on kubernetes, when the msg resource is mapr-stream"
menu = ""
banner = "https://app.yinxiang.com/shard/s33/res/d2e0b66b-1525-4965-8700-ef5f7ebe4cd7/PSX_20191018_225433.jpg"
+++

THe How-to guids to set up kafka python applications on kubernetes, when the msg resource is mapr-stream

## Architecture
![Mapr Client](https://app.yinxiang.com/shard/s33/res/c437573e-54e5-46ea-af27-81dfc619d381/Screenshot%20from%202019-10-15%2015-12-59.png)
1. MapR core
2. The MapR-ES Python Client is a binding for librdkafka that is dependent on the MapR-ES C client (MapR-ES C Client is a distribution of librdkafka that works with MapR-ES).
3. The mapr-streams-python package replace the normal confluent-python package. 
    
 - As of MEP 5.0 MapR-ES Python Client: 
    ```
    from confluent_kafka import Producer
    p = Producer({'streams.producer.default.stream': '/my_stream'})
    some_data_source= ["msg1", "msg2", "msg3"]
    for data in some_data_source:
         p.produce('mytopic', data.encode('utf-8'))
         p.flush()
    ```
    
 - As of MEP 3.0 MapR-ES Python Client
    ```
    from mapr_streams_python import Producer
    p = Producer({'streams.producer.default.stream': '/my_stream'})
    some_data_source= ["msg1", "msg2", "msg3"]
    for data in some_data_source:
        p.produce('mytopic', data.encode('utf-8'))
        p.flush()
    ```
## Prerequisites

0. MapR client: base
1. MapR-ES C Client (mapr-librdkafka)
2. GNU Compiler Collection (GCC) is installed on the node.
3. Python 2.7.1 and above or Python 3.6.7
4. Python pip
5. python-devel 
6. valid mapr-ticket
....

## Step by Step

1.build docker file 

see the example dockerfile

	````
	FROM ubuntu:18.04

	ARG INDEX_URL_WITH_ACCESS
	ARG workdir=/home/appuser

	COPY config/ .

	RUN mv ubuntu-sources.list /etc/apt/sources.list \
	    && apt-get update \
	    && apt-get upgrade -y \
	    && apt-get install -y gpg

	RUN mv 3rdparty.list /etc/apt/sources.list.d/ \
	    && apt-key add 3rdparty.key \
	    && apt-get autoremove --purge -y gpg \
	    && apt-get update \
	    && rm -f 3rdparty.key

	RUN apt-get update \
	    && apt-get install -y openjdk-8-jdk --fix-missing \   #needed for mapr client
	    && apt-get install -y mapr-client --fix-missing\
	    && apt-get install -y mapr-librdkafka --fix-missing\
	    && apt-get install -y mapr-patch-client --fix-missing\
	    && apt-get install -y gcc --fix-missing --fix-missing\ #needed for c lib: librdkafka
	    && apt-get install -y mtools --fix-missing

	RUN apt-get update \
	    && apt-get install -y python3 python-dev python3-dev python3-pip --fix-missing

	RUN apt-get install -y libssl1.0.0

	ADD http://package.mapr.com/releases/MEP/MEP-6.1/ubuntu/mapr-kafka_1.1.1.201901241010_all.deb /tmp
	RUN apt-get install /tmp/mapr-kafka_1.1.1.201901241010_all.deb && rm -rf /tmp/mapr-kafka_1.1.1.201901241010_all.deb

	# switch to app user and working dir
	WORKDIR $workdir

	# install tools and add user
	RUN pip3 install --upgrade \
	    pip \
	    setuptools \
	    wheel \
	&& groupadd --gid 1000 appuser \
	&& useradd --uid 1000 --gid 1000 --home $workdir appuser \
	&& chown -R appuser $workdir

	USER appuser
	# copy src
	COPY --chown=appuser:appuser . .

	# install your own app dependencies and remember to remove the normal confluent-kafka
	RUN pip3 install . --extra-index-url=$INDEX_URL_WITH_ACCESS --user && echo "y" | pip3 uninstall confluent_kafka

	# install the mapr-streams-python as root
	USER root
	RUN pip3 install --global-option=build_ext --global-option="--library-dirs=/opt/mapr/lib" --global-option="--include-dirs=/opt/mapr/include/" mapr-streams-python

	USER appuser
	# important: set the path
	ENV LD_LIBRARY_PATH=/opt/mapr/lib
	ENV JAVA_HOME=/usr/bin/java

	# bind app to port
	EXPOSE 8888

	# start app
	CMD [ "python3", "-u", "index.py" ]

	````

2.Docker push

3.Create a image pushll secret to pull from Artifactory
    if you pull the image from private artifactory

4.Build the Kuberneter deployment yaml file
  
  ```
     spec:
      securityContext:
        runAsUser: 1000
        runAsGroup: 1000
      imagePullSecrets:
        - name: your-registry-secret
      containers:
        - name: data-handler-api
          image: path/to/your/images
          args:
            - sleep
            - "180"
          ports:
             - containerPort: 7222
             - containerPort: 5181
          env:
            - name: MAPR_TICKETFILE_LOCATION
              value: "/path-to-the-mapr-ticket"
          envFrom:
            - secretRef:
                name: your-app-credentials-of
          volumeMounts:
            - name: mapr-secret-volume
              mountPath: /opt/mapr/conf/ssl_truststore
              subPath: ssl-truststore
            - name: mapr-config-volume
              mountPath: /opt/mapr/conf/mapr-clusters.conf
              subPath: mapr-clusters
      volumes:
        - name: mapr-secret-volume
          secret:
            secretName: mapr-secret
        - name: mapr-config-volume
          configMap:
            name: mapr-config
  ```
5.create secret with content pf your with the content of /opt/mapr/conf/ssl_truststore

 ```commandline    
    cat /opt/mapr/conf/ssl_truststore | base64
  ```
and put the the content into mapr-secret.yaml

        apiVersion: v1
        kind: Secret
        metadata:
          name: mapr-secret
        data:
          ssl-truststore: |
     <base64 encoded content of /opt/mapr/conf/ssl_truststore>

create kubernetes secret
    ```commandline    
    	kubectl apply -f mapr-secret.yaml
    ```

 6.the same for Configmap
 
	 ```
	apiVersion: v1
	kind: ConfigMap
	metadata:
	  name: mapr-config
	data:
	  mapr-clusters: |
	    all the clusters node
	 ```
create kubernetes config mapr
  ```commandline    
 kubectl apply -f config-map.yaml
  ```

7.Login the pod and subscribe
 > kubectl exec -it pod-name bash -n namespace

8.Login to mapr

```
 kafka-user@kafka-client:/$ maprlogin password -user <userId>
```
9.Test connection of mapr client
> hadoop fs -ls /
> mapr streamanalyzer -path <stream-full-name>


## Debugging

1.  set the config of consumer to debug mode:

 ```
        kafka_native_config = {
        "group.id":"sv_prod",
        "default.topic.config": {"auto.offset.reset": "earliest"},
        "debug":"cfdpg" # your can use"all,"broker", "consumer","producer"..
    }
 ```

## Trouble shooting
1. no connection:
- reason & solution: 
    don't install normal `confluent-python` package, you should install the "special version" of it, called mapr-streams-python
  

## Reference
1. https://mapr.com/docs/60/MapR_Streams/MapRStreamsPythonApplications.html
2. https://mapr.com/docs/60/MapR_Streams/MapRStreamsPythonExample.html



