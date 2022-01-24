+++
title = "Errors and Solutions with Hyperledger Fabric"
date =  2018-06-21T16:22:23+02:00
tags = ["hyperledger-fabric","blockchain","debug"]
categories = ["tech"]
description = "I'm an avid open-source supporter, and have always been thinking that open-source projects overweight than most of similar project, until recently I encounters countless errors and problems with Hyperledger Fabric, I started to think the limitations of open-source software"
menu = ""
banner = "banners/fabric-logo.png"
+++

Hyperledger fabric is not stable and robust as we thought, usually you may find out conflicts of versions, disappearing docker images, invalid links, unclear descriptions, shabby documentary.
like one of the bloger said:
```
	I have faced “n” number of errors while I was working in Blockchain project, I have spent countless hours reading out articles, reaching out experts to fix the same. This article is to just help the people who are beginners & struggling to solve the errors that are quite often pop up during your development stage.
```
So I'd love to share my experience in debugging.

## Error No.1
```
DEBU 003 grpc: addrConn.resetTransport failed to create client transport: connection error: desc = "transport: Error while dialing dial tcp: lookup orderer.example.com on 127.0.0.11:53: no such host"; Reconnecting to {orderer.example.com:7050 <nil>}

```
### Reason: In most cases, the orderer failed to setup.
    
use `docker ps` to take a look:
```
CONTAINER ID        IMAGE                             COMMAND             CREATED             STATUS              PORTS                                            NAMES
d0b84b7aa41e        hyperledger/fabric-tools:latest   "/bin/bash"         31 seconds ago      Up 29 seconds                                                        cli
13d174121481        hyperledger/fabric-peer:latest    "peer node start"   36 seconds ago      Up 32 seconds       0.0.0.0:9051->7051/tcp, 0.0.0.0:9053->7053/tcp   peer0.Distributor.example.com
e959d8a0b974        hyperledger/fabric-peer:latest    "peer node start"   36 seconds ago      Up 31 seconds       0.0.0.0:6051->7051/tcp, 0.0.0.0:6053->7053/tcp   peer0.Retailer.example.com
aa0ababa9028        hyperledger/fabric-peer:latest    "peer node start"   36 seconds ago      Up 33 seconds       0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp   peer0.Operator.example.com
ba5fa6d6c8ff        hyperledger/fabric-peer:latest    "peer node start"   36 seconds ago      Up 30 seconds       0.0.0.0:8051->7051/tcp, 0.0.0.0:8053->7053/tcp   peer0.Supplier.example.com
``` 
### Solution 1:
check the location info written in `docker-compose-base.yml` and any file containing the relevant infomation:

```
 volumes:
    - ../channel-artifacts/genesis.block:/var/hyperledger/orderer/orderer.genesis.block
    - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/msp:/var/hyperledger/orderer/msp
    - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/:/var/hyperledger/orderer/tls
```

the absolute path with file, we should add the quotation marks:

```
- "../channel-artifacts/genesis.block:/var/hyperledger/orderer/orderer.genesis.block"
```

### Solution 2:
If still unsolved, we should try to read the log and find out. Here's the way to find error in `logs`.
We try to setup the orderer individually by creating a new config file `docker-compose-li-test.yaml` with the content:

```
# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#

version: '2'

services:

  orderer.example.com:
    extends:R
      file:   base/docker-compose-base.yaml
      service: orderer.example.com
    container_name: orderer.example.com
``` 
and run the respectively command

```
docker-compose -f docker-compose-cli-test.yaml up
```
Naja, I found another new bug, it's like Recursion... So I have to find what

```
orderer.example.com    | 	Kafka.Topic.ReplicationFactor = 3
orderer.example.com    | 	Debug.BroadcastTraceDir = ""
orderer.example.com    | 	Debug.DeliverTraceDir = ""
orderer.example.com    | 2018-05-31 19:40:14.823 UTC [orderer/common/server] initializeServerConfig -> INFO 003 Starting orderer with TLS enabled
orderer.example.com    | 2018-05-31 19:40:14.850 UTC [fsblkstorage] newBlockfileMgr -> INFO 004 Getting block information from block storage
orderer.example.com    | 2018-05-31 19:40:14.882 UTC [orderer/commmon/multichannel] newLedgerResources -> PANI 005 Error creating channelconfig bundle: initializing channelconfig failed: could not create channel Orderer sub-group config: setting up the MSP manager failed: the supplied identity is not valid: x509: certificate signed by unknown authority (possibly because of "x509: ECDSA verification failure" while trying to verify candidate authority certificate "ca.example.com")
orderer.example.com    | panic: Error creating channelconfig bundle: initializing channelconfig failed: could not create channel Orderer sub-group config: setting up the MSP manager failed: the supplied identity is not valid: x509: certificate signed by unknown authority (possibly because of "x509: ECDSA verification failure" while trying to verify candidate authority certificate "ca.example.com")
orderer.example.com    | 
```

What works for me is the following method:

```
# remove previous crypto material and config transactions
rm -fr channel-artifacts/*
rm -fr crypto-config/*
```
## Error No.2: channel creation failed

```
Error: got unexpected status: BAD_REQUEST -- Attempted to include a member which is not in the consortium
```
### Reason:  failed to set the channel name. 

### Solution: 
```
export CHANNEL_NAME=mychannel
peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```