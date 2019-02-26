+++
title =  "Running First Hyperledger Fabric Sample"
date =  2018-05-15T13:57:55+01:00
tags = ["Blockchain","Hyperledger Fabric"]
categories = ["tech"]
description = "This is some annotation about running the sample"
menu = ""
banner = "banners/bread.JPG"
+++

# Hyperledger Fabric Sample Application
This is originally from the `Introduction to Hyperledger Technologies` course on edX. [Link](https://www.edx.org/course/blockchain-business-introduction-linuxfoundationx-lfs171x)
However, if follow the instructions, you may not successfully run the example.

the overview graph
![clikck](https://user-images.githubusercontent.com/10289229/43036680-59b85d68-8d06-11e8-99d3-66b87f2f4b48.png)

# Node.js version
When an error occurs, in most case it has something to do with the version of Node.js's version


**completely remove Node.js old version**
completely uninstall node + npm is to do the following:

1. go to /usr/local/lib and delete any node and node_modules
2. go to /usr/local/include and delete any node and node_modules directory
check your Home directory for any local or lib or include folders, and delete any node or node_modules from there
3. go to /usr/local/bin and delete any node executable

```
sudo rm -rf /opt/local/bin/node /opt/local/include/node /opt/local/lib/node_modules
sudo rm -rf /usr/local/bin/npm /usr/local/share/man/man1/node.1 /usr/local/lib/dtrace/node.d
```


**install Node.js v8.x:**

```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs

# Using Debian, as root
curl -sL https://deb.nodesource.com/setup_8.x | bash -
apt-get install -y nodejs

```


# npm install error

you may encounter those errors like the following:

```
npm WARN package.json range-parser@0.0.4 No repository field.


```
you have modify the setting in package.json: 
```
{"name": "Fabric-tuna-app",
    "version": "1.0.0",
    "private": true,
    ...
}
```

# reference
1. [npm WARN package.json: No repository field](https://stackoverflow.com/questions/16827858/npm-warn-package-json-no-repository-field)
