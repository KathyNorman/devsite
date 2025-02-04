# NULS ChainBox
## What’s NULS ChainBox

ChainBox is an out-of-the-box solution to chain-building. It encapsulates six underlying modules: ledger, accounts, transactions, blocks, consensus, and network. ChainBox eliminates the necessity of direct blockchain experience and the need to understand distributed data storage, point-to-point transmission, consensus mechanisms, and encryption algorithms. Instead of spending time on blockchain architecture, developers can focus on creating independent business modules based on standard communication protocols, then form a brand new application chain in minutes with ChainBox.

## Why Chainbox

NULS ChainBox is designed to help enterprises or application developers build applications on the blockchain with ease and focus on their own business implementations without concern underlying complex technology of blockchains.

## Features
Essentially NULS ChainBox is an extension of NULS 2.0 position: to be a one-stop blockchain development platform. It has three core features:
- Quick set up development environment.
- Lower the threshold of application development by using templates.
- Reduce the difficulty of integrating with NULS provided templates by utilizing provided scripts and one-click generation of executable programs.

## Quick start
In the following ChainBox example, you will see how to quickly build a blockchain application that provides an encrypted message services.

### 1 Environment preparation
- Linux or MacOS
- Git
- Maven
- JDK11
Java must be set in the $PATH environment variable.

### 2 Get NULS ChainBox Repository
Open the terminal and run the following command

```
git clone https://github.com/nuls-io/nuls-chainbox.git chainbox
```
### 3 Building encrypted message module
Change directory to the `example` directory:
```
cd example   # enter the example folder
```
Run the command to build a example module:

```
package    # execute the build script (provided by the template)
```
The following output indicates the success of the build
```
============ PACKAGE FINISH 🍺🍺🍺🎉🎉🎉 ===============
```

If the build is successful, you will find the `outer` folder generated in `example`.

> PS: If you want to know the detailed design of the module, please refer to the [message Module Design](./developeModule.md)

### 4 Integrate the encrypted message module into chainbox
Go back to the `chainbox` root directory:

```
cd ..
```

Run the command to integrate the encryption module into the NULS 2.0 runtime environment:

```
tools -p example
```
The following output indicates the success of the integration:

```
============ PACKAGE FINISH 🍺🍺🍺🎉🎉🎉 ===============
```

The `NULS-WALLET` folder will be created in the `chainbox` directory. It contains the NULS 2.0 running program integrated with the encrypted message module, and support directories such as Logs, and scripts.

If multiple nodes are deployed on different machines, it is recommended to modify the following two parameters in the `NULS-WALLET/.default-config.ncf` configuration file.

 ```
# Minimum number of linking nodes. If the number of linking nodes is lower than this parameter, it will continue to wait.
minNodeAmount=0

# Seed node block address
seedNodes=tNULSeBaMkrt4z9FYEkkR9D6choPVvQr94oYZp
```




### 5 Start your node!
Execute the following command in the `NULS-WALLET` directory:

```
start-dev
nuls.ncf is created by default-config.ncf.
Please re-excute the startup program.
```

The nuls.ncf is created during the first execution of the command. Re-enter the command:

```
start-dev
```
The following output indicate that the chainbox modules are starting:

```
LOG PATH    : ~/NULS-WALLET/Logs
DATA PATH   : ~/NULS-WALLET/data
CONFIG FILE : ~/NULS-WALLET/nuls.ncf
DEBUG       : 0
JAVA_HOME   : /Library/java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
java version "11.0.2" 2019-01-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)

====================
NULS-WALLET STARTING
====================
```

Check the startup status using the following command:

```
check-status
```
The following output indicate that all the modules of the node have been successfully started. Notice the module 'mail-example' is included.
```
==================MODULE PROCESS====================
account PROCESS IS START
block PROCESS IS START
consensus PROCESS IS START
ledger PROCESS IS START
network PROCESS IS START
transaction PROCESS IS START
==================RPC READY MODULE==================
account RPC READY
block RPC READY
consensus RPC READY
ledger RPC READY
network RPC READY
transaction RPC READY
======================READY MODULE==================
account STATE IS READY
block STATE IS READY
consensus STATE IS READY
ledger STATE IS READY
network STATE IS READY
transaction STATE IS READY
================TRY RUNNING MODULE==================
account TRY RUNNING
block TRY RUNNING
consensus TRY RUNNING
ledger TRY RUNNING
network TRY RUNNING
transaction TRY RUNNING
===================RUNNING MODULE===================
account STATE IS RUNNING
block STATE IS RUNNING
consensus STATE IS RUNNING
ledger STATE IS RUNNING
network STATE IS RUNNING
transaction STATE IS RUNNING
==================NULS WALLET STATE=================
==========================
NULS WALLET IS RUNNING
==========================
```

### 6 Start creating blocks, and other housekeeping

Now that the node is started, also referred to as the 'seed node', we need to import the default block address of the seed node so that the node can begin to produce blocks.

First start the command line interface:

First enter the command line

```
cmd
...
Module:cmd-client
...
waiting nuls-wallet base module ready
nuls>>>
```
Next you must import the block address of the seed node.  This is very important. If you miss this step, or enter the wrong details, the seed node will not begin building blocks and 'mail-example' will not work.

Enter:   import b54db432bba7e13a6c4a28f65b925b18e63bcb79143f7b894fa735d5d3d09db5
and, when prompted, provide the Password: nuls123456

```
nuls>>> import b54db432bba7e13a6c4a28f65b925b18e63bcb79143f7b894fa735d5d3d09db5
Please enter the password (password is between 8 and 20 inclusive of numbers and letters), If you do not want to set a password, return directly.
Enter your password:**********
Please confirm new password:**********
tNULSeBaMkrt4z9FYEkkR9D6choPVvQr94oYZp

# The password for the imported address  must be nuls123456
# You will be asked to enter the password twice
```
The returned NULS account address is 'tNULSeBaMkrt4z9FYEkkR9D6choPVvQr94oYZp'.
Note: 'import address' is successful if the returned address is the same as the 'Seed Node Block Address' listed as a configuration item is Step 4.

### 7 Get nuls accounts for 'mail-example'
Setup two accounts to test sending and receiving a message. The following two addresses are pre-defined in the genesis block.  For these accounts, enter any password.

```
nuls>>> import 477059f40708313626cccd26f276646e4466032cabceccbf571a7c46f954eb75
Please enter the password (password is between 8 and 20 inclusive of numbers and letters), If you do not want to set a password, return directly.
Enter your password:**********
Please confirm new password:**********
tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD

nuls>>> import 8212e7ba23c8b52790c45b0514490356cd819db15d364cbe08659b5888339e78
Please enter the password (password is between 8 and 20 inclusive of numbers and letters), If you do not want to set a password, return directly.
Enter your password:**********
Please confirm new password:**********
tNULSeBaMrbMRiFAUeeAt6swb4xVBNyi81YL24
```
For testing 'mail-example' the accounts 'tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD' and 'tNULSeBaMrbMRiFAUeeAt6swb4xVBNyi81YL24' are now available.

### 8 Testing 'mail-example'

Enter the `NULS-WALLET/Modules/Nuls/mail-example/1.0.0` directory and open `mail-test.html` with a browser (this is a simple test page that can test the functions of 1.) associating a sender/receiver with a NULS address, 2.) sending a message, and 3.) viewing the sent message.

First, associate the sender/receiver with a NULS address. The sender/receiver can be an email address, as in the example that follows, or any other identifier such as 'kathy' and 'bella'.

Set the sender/receiver for one NULS account, and enter the password of the  account you used. The hash value of this transaction will be returned if the submission is successful.  Then, do the same for the second NULS account.

In the example that follows, asd@nuls.io and l24@nuls.io are used. asd@nuls.io and l24@nuls.io are each associated with a NULS account.

Then, send a message to 24@nuls.io from asd@nuls.io. Enter the receiver's address (asd@nuls.io), the sender's account address, and the sender's account password. The hash value of this transaction will be returned if the submission is successful.

After waiting for about 10 seconds (to ensure that the transaction has been confirmed), the content of the message can be viewed by the hash value of the message sent. Only the sender and the receiver have the permission to view it.

Here is the decrypted sent message, as provided by 'mail-example':
  ```
  {
      "sendermessageAddress": "asd@nuls.io",   //Sender's messagebox address
      "receivermessageAddress": "24@nuls.io",  /Receiver’s messagebox address
      "title": "this is title",             //message title
      "content": "NULS 666.",               //message content
      "sender": "tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD",   //Sender’s account address
      "date": 1561365228904                 //Delivery timestamp (milliseconds from January 1, 1970 to the present)
  }
  ```

## ChainBox Guide
### Directory Structure of ChainBox
#### tools
The script tools downloads packages and repositories, integrates packages, etc.
[Command Parameter Document](#cmd-doc).
#### document
The directory document contains the documentation for the six core modules that comprise the basic block chain program. Currently these documents are in Chinese.  If you are interested in finding out more about these topics, we suggest you use Google Translate.
#### example
An example module program source code for an encrypted mail developed based on a java module template. The example directory includes source code, test.html, package script, module.ncf, README.md.
#### rpc-debug-tool
Basic module rpc interface debugging tool.

#### NULS_WALLET: the NULS2.0 run environment

The NULS2.0 running environment containing all necessary files for the contains the most basic block chain program, which are the six core modules: account, ledger, block, network, transaction, consensus (poc).

To add additional functionality, such as cross chain transfer, the configuration file, nuls.ncf, must be modified and NULS_WALLET must be rebuilt. ([complete configuration list](https://github.com/nuls-io/nuls-v2/blob/develop/useguide.md#nulsncf-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)).

To integrate your own business module(s) in chainbox, the NULS2.0 run environment is modified using the script 'tools'. More information is provided in the next section.

##### tools
The script 'tools' is used to download templates and repositories, and to build the NULS2.0 run environment.

```
tools -h
Desc: NULS-engine script
Usage: ./tools
   -t <language> [folder name] Get the module program module of the specified development language
<language> development language, such as java
[folder name] Download to the directory with the specified name
   -r <folder name> Retrieve the business module code and execute the package to compile
   -l View a list of supported templates
   -n [base module name...] Get the NULS2.0 runtime environment (pull from the nuls-v2/nuls-engine branch) By default, 6 core modules are built, and optional modules can be added by passing in optional module names.
   -p [business module directory...] build NULS2.0 wallet
   -a add a base module
   -d remove a base module
   -s View the configured list of base modules to be packaged
   -i <package.ncf> specifies the configuration file of the base module to be packaged
   -o <folder path> specifies the output directory
   -h View help
```
The following '-n' 'tools' option checks the current environment, then pulls NULS2.0 code from the github repository, executes 'package' to complete the NULS2.0 compilation and packaging, and writes the runnable program and support directories to the NULS_WALLET directory.

```
tools -n
```
When you see the following, 'tools -n' has successfully completed:
```
============ .../.NULS2.0/NULS-WALLET-RUNTIME PACKAGE FINISH 🍺🍺🍺🎉🎉🎉 ===============
```
To start and complete the configuration of chainbox, execute steps 5 and 6 of the 'Quick Start' section.

#### NULS-WALLET directory structure
##### start-dev
- Start node.
##### stop-dev
- Stop node.
##### check-status
- Check the operating status of each module.
##### cmd
- Command line startup script.
##### create-address
- Create NULS account and return account address and private key.

```
./create-address
JAVA_HOME:/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
java version "11.0.2" 2019-01-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)

.../NULS_WALLET/nuls.ncf
Service Manager URL: ws://127.0.0.1:7771
chainId:2
number:1
====================================================================================================
address   :tNULSeBaMtrzD4kChZPfEAFLRqX18n7UrrbY1s
privateKey:f67877e994d6bd0ef23e0aaf0f8c6b62edf5dc636ff5b6981666c4632499bc56
====================================================================================================
```


##### nuls.ncf
- Configuration file (created after running the start-dev script for the first time).
##### More usage reference ([NULS2.0 Wallet User Manual](https://github.com/nuls-io/nuls-v2/blob/develop/useguide.md))

### How to develop your own module (also called 'Business Module')
NULS2.0 is a distributed microservice architecture  written in the JAVA language. It consists of multiple modules, each of which communicates via the websocket protocol. NULS 2.0 defines a standard [module communication protocol](https://github.com/nuls-io/nuls-v2-docs/blob/master/design-zh-CHS/r.rpc-tool-websocket%E8%AE%BE%E8%AE%A1v1.3.md), which can be implemented in various development languages to communicate with other modules.
To interact with the blockchain your business module registers it's transaction type(s) at startup.  Your business module  stores its own business data in the transaction's txData, and  passes the assembled transaction  to the transaction module which will manage the verification of the transaction, and, if correct, stores the transaction in a packed queue which is subsequently processed by the block module.
#### Creating a transaction process
![node creation transaction](./chainbox/createTX.jpg)
#### Processing network transaction process
![Processing Webcast Transactions](./chainbox/network.jpg)

As you can see from the figure, there are four main things you need to do:
1. Register your own transaction type to the blockchain. A transaction type represents a unique write to the blockchain. For example,  mail-example has 2 types: a. create mail address, where details about the mail address are written to the ledger, b. send mail, where the details about the email message are written to the ledger.).
2. Assemble the transaction data and create the new transaction.
3. Verify that the business data in the transaction is valid.
4. Save the business data in the transaction to the node database.

Of course, in addition to the above 4 steps, business data needs to be used according to specific business needs.

Here are the above 4 steps in detail.

Each type of transaction in the business module is  registered with the transaction module with a unique transaction value, called transaction type.  These values are 200 and above (integer). The value needs to be unique within this occurrence of chainbox.  The transaction type is used to distinguish the callback function of the processed transaction. The registration transaction interface provided by the transaction module is called when the business module is started (please refer to 'mail-example'). When the transaction module gets a transaction to be processed, the transaction, including the transaction data and business data, will be verified according to the transaction type. In addition to verification, there are two functions: commitTx (save transaction business data) and rollbackTx (rollback transaction business data).

The contents of a transaction include transaction type, timestamp, CoinData, txData, remarks, and signature. CoinData includes transfer data, transfer accounts, transfer accounts, transfer amounts, asset information, and so on. The txData contains business data. The business module stores its own business data in txData according to the business design. The signature field signs all transaction data by an elliptic curve algorithm to ensure that the data is not skewed during transmission. After the assembly is complete, the transaction module interface is called to create the transaction.

When the transaction module gets the transaction, it checks whether the parameters of the transaction data is correct, then checks whether the account balance is sufficient to pay the transaction fee, and then verifies the nonce value of the account (using an algorithm that guarantees that the balance is not reused by controlling the transaction order ). After the verification is passed, the callback function of the business verification is found according to the transaction type, and the transaction business data is verified.

Finally, when the transaction is added to the block and the block has been confirmed, a callback function for storing the business data will be found through the transaction type, and the business module is notified to ‘backup’ the data. In some cases, block rollback may occur. When the block is rolled back, the transaction data is also matched to the corresponding transaction rollback callback function to roll back the business data.

The above are the core steps required for  new transaction type. The three interfaces for verifying transactions, saving business data, and rolling back business data are implemented by the business module.

### Communicate with other modules
NULS 2.0 uses a microservices architecture, and modules communicate using [websocket](https://zh.wikipedia.org/wiki/WebSocket). The links to all modules are governed by Nulstar and the process is as follows:

![](./chainbox/pic01.jpg)

All modules are started by the ServiceManager. After startup, each module (also referred to as service module)  establishes a connection with the ServiceManager module. This handshake is completed according to protocol. After the handshake is successful, the module is registered with the ServiceManager. The purpose of the registration is to tell the ServiceManager its connection mode, the provided interface protocol, and which modules it relies on.
#### Establish connection
The connection is established using the standard websocket protocol. After the connection is established, the module can send a packet (interface request) to any module and receive the other party's data packet. Note: All requests are asynchronous  non-blocking.
#### Handshake with ServiceManager
After establishing a connection with the ServiceManager, the module sends a NegotiateConnection object. The module can process other requests only when the negotiation is successful. Otherwise, the module receives the NegotiateConnectionResponse message with the status set to 0 (failed) and immediately disconnect.
The NegotiateConnection message consists of two fields:

- CompressionAlgorithm (default: zlib): A String representing the algorithm that will be used to receive and send messages if CompressionRate is greater than zero. The default is zlib, which is supported in most development languages.
- CompressionRate: An integer between 0 and 9 that establishes the level of compression at which messages should be sent and received for this connection. 0 means no compression, and 9 is maximum compression.




Example:

```json
    {
        "MessageID": "15622130397455",
        "Timestamp": "1562213039745",
        "TimeZone": "9",
        "MessageType": "NegotiateConnection",
        "MessageData": {
            "Abbreviation": "ledger",
            "ProtocolVersion": "0.1",
            "CompressionAlgorithm": "zlib",
            "CompressionRate": "0"
        }
    }
```
After the handshake is successful, the ServiceManager sends a NegotiateConnectionResponse object to the service module. It consists of two fields:

- NegotiationStatus: Unsigned small integer value, 0 if negotiation failed, 1 if successful
- NegotiationComment: A string value that describes what went wrong when rejecting the connection.

Example:

```json
{
    "MessageID": "156221303974612033",
    "Timestamp": "1562213039759",
    "TimeZone": "9",
    "MessageType": "NegotiateConnectionResponse",
    "MessageData": {
        "RequestID": "15622130397455",
        "NegotiationStatus": "1",
        "NegotiationComment": "Connection true!"
    }
}
```

#### registerAPI (registration module)
After the handshake with the ServiceManager is successful, the RegisterAPI request is sent by the registering module to the ServiceManager to complete the registration. The ServiceManager will obtain the connection information and interface method of the module through this request. The ServiceManager determines whether the registering module meets the normal working conditions by analyzing whether it's provided list of "dependency" module(s) exist. (See example.)

Example:

```
{
    "MessageID": "15622130392482",
    "Timestamp": "1562213039248",
    "TimeZone": "9",
    "MessageType": "Request",
    "MessageData": {
        "RequestAck": "0",
        "SubscriptionEventCounter": "0",
        "SubscriptionPeriod": "0",
        "SubscriptionRange": "0",
        "ResponseMaxSize": "0",
        "RequestMethods": {
            "RegisterAPI": {
                "Methods": [
                    {
                        "MethodName": "getAssetsById",
                        "MethodDescription": "清除所有账户未确认交易",
                        "MethodMinEvent": "0",
                        "MethodMinPeriod": "0",
                        "MethodScope": "public",
                        "Parameters": [
                            {
                                "ParameterName": "chainId",
                                "ParameterType": "",
                                "ParameterValidRange": "[1-65535]",
                                "ParameterValidRegExp": ""
                            },
                            {
                                "ParameterName": "assetIds",
                                "ParameterType": "",
                                "ParameterValidRange": "",
                                "ParameterValidRegExp": ""
                            }
                        ]
                    }
                ],
                "Dependencies": {
                    "block": "1.0",
                    "network": "1.0"
                },
                "ConnectionInformation": {
                    "IP": "192.168.0.197",
                    "Port": "17880"
                },
                "ModuleDomain": "Nuls",
                "ModuleRoles": {
                    "ledger": [
                        "1.0"
                    ]
                },
                "ModuleVersion": "1.0",
                "Abbreviation": "ledger",
                "ModuleName": "ledger"
            }
        }
    }
}
```
When the ServiceManager determines that the dependency modules have been started, it returns a response to the registering module that contains the way the dependent modules are linked.

Example:

```
{
    "MessageID": "1562213039283455",
    "Timestamp": "1562213039283",
    "TimeZone": "9",
    "MessageType": "Response",
    "MessageData": {
        "RequestID": "15622130392482",
        "ResponseProcessingTime": "2",
        "ResponseStatus": 0,
        "ResponseComment": "success",
        "ResponseMaxSize": "0",
        "ResponseData": {
            "RegisterAPI": {
                "Dependencies": {
                    "consensus": {
                        "IP": "192.168.0.197",
                        "Port": "18735"
                    },
                    "ledger": {
                        "IP": "192.168.0.197",
                        "Port": "17880"
                    },
                    "nuls-module-explorer": {
                        "IP": "192.168.0.197",
                        "Port": "10130"
                    },
                    "kernel": {
                        "IP": "0.0.0.0",
                        "Port": "7771"
                    },
                    "block": {
                        "IP": "192.168.0.197",
                        "Port": "13437"
                    },
                    "transaction": {
                        "IP": "192.168.0.197",
                        "Port": "14026"
                    },
                    "account": {
                        "IP": "192.168.0.197",
                        "Port": "15121"
                    },
                    "network": {
                        "IP": "192.168.0.197",
                        "Port": "15481"
                    }
                }
            }
        },
        "ResponseErrorCode": null
    }
}
```
***After the registering module obtains the link mode of the dependent module, it can establish a connection with  other business module(s) and obtain the connected business module's interface that is available for other business modules. *** is this clear and complete? ***

#### Calling other module interfaces
Before calling the interface of other modules, it is also necessary to complete the operation of establishing a websocket connection and shaking hands with the module. After the handshake is completed, the Request  is sent to the module.

Example:

```
{
    "MessageID": "156222086356134",
    "Timestamp": "1562220863561",
    "TimeZone": "9",
    "MessageType": "Request",
    "MessageData": {
        "RequestAck": "0",
        "SubscriptionEventCounter": "0",
        "SubscriptionPeriod": "0",
        "SubscriptionRange": "0",
        "ResponseMaxSize": "0",
        "RequestMethods": {
            "ac_createAccount": {
                "chainId": 2,
                "count": 1,
                "password": "nuls123456"
            }
        }
    }
}
```
The content of RequestMethods is the requested parameter, and the outer data is the protocol layer. After the business module is processed, the Response object is sent, and the processing result and the result data are returned.

Example:

```
{
    "MessageID": "156222086367037",
    "Timestamp": "1562220863664",
    "TimeZone": "9",
    "MessageType": "Response",
    "MessageData": {
        "RequestID": "156222086356134",
        "ResponseProcessingTime": "107",
        "ResponseStatus": 0,
        "ResponseComment": "success",
        "ResponseMaxSize": "0",
        "ResponseData": {
            "ac_createAccount": {
                "list": [
                    "tNULSeBaMkef3L6EsMcigwT1C1uebzfsj63jd3"
                ]
            }
        },
        "ResponseErrorCode": null
    }
}
```
#### Reference Document:
* [Websocket-Tool Design Document](https://github.com/nuls-io/nuls-v2-docs/blob/master/design-zh-EN/r.rpc-tool-websocket%E8%AE%BE%E8%AE%A1v1.3.md)
* [Nulstar Module Specification](https://github.com/nuls-io/Nulstar/blob/master/Documents/Nulstar%20-%20Documentation%20-%20Module%20Specification.pdf)
* [Basic Module Interface Document](#doclist)

### Get module development templates for various development languages
In theory, as long as the connection is established between the business module and another module using the websocket, information can be exchanged between the two modules according to the agreed protocol. In order to reduce the difficulty of module development, NULS provides quick start templates for java, with the intention of providing other language templates in the future.  Developers only need to use a template. The development of the business module can be completed by inserting specific business logic code at  specified locations in the template.

All template can be listed using the tools script.

```
tools -l
java-module
nuls-module-explorer
nuls-module-sdk-provider
nuls-module-web-wallet
nuls-module-helloworld
```

A template can be downloaded using:

```
tools -t java-module
```
After the execution is completed, the directory 'java-module' is created in the current directory, and the common development tools can be imported to start the development business. There will be corresponding usage documents in each template.
### Module debugging method
In the module development process, you need to coordinate with the chainbox. After obtaining the NULS2.0 runtime environment, execute the start-mykernel script (start-dev) to start the NULS2.0 basic module. The Service Manager is accessible via URL: ws://127.0.0.1:7771. The  developing module will register with the Service Manager. After completing the registration, the developing module can get the communication address of each dependent module and call each module's interface. The program is deployed to production when finished; the business module needs to be integrated into the NULS2.0 runtime environment. Then the entire package is deployed to the production environment which may include other external nodes.
1. The packaged executable program should be placed in the outer directory of the module development directory.
2. The outer directory must have a configuration file named Module.ncf (note M capitalization). The contents of the file are as follows (using java as an example):

```
[Core]
Language=JAVA # Indicate the development language
Managed=1 # 1 means the module starts with the node program, 0 means manual start

[Libraries]
JRE=11.0.2 # Module Operating Environment Version

[Script]
Scripted=1 # Whether to use script to start 1 means yes
StartScript=start # Start the module script (start must be in the outer directory)
StopScript=stop # Stop the module script (stop must be in the outer directory)

```
3. The module can be started and stopped by the script configured in 2

> The above convention has been completed in the module development template.

## Appendix
### <span id="cmd-doc">tools script manual</span>
### Getting the NULS2.0 runtime environment
#### Command: tools -n
#### parameter list
no
#### Example

```
tools -n
```
### Get the specified language module development template
#### Command: tools -t &lt;language> [out folder]
#### parameter list
| Parameter | Description |
| --- | --- |
| &lt;language> | Language Template Name |
| [out folder] | Output folder name |
#### Example

```
tools -t java-demo
```
### View a list of available templates
#### Command: tools -l
#### parameter list
no
##### Example

```
tools -l
```
### Integrating modules into the NULS 2.0 runtime environment
#### Command: tools -p &lt;module folder>
#### parameter list
| Parameter | Description |
| --- | --- |
| &lt;out folder> | Module folder name |
#### Example

```
tools -p mail-example nuls-helloworld-template
```
### <span id="registerTx">Business Module Related Interface Protocol</span>
The business module needs to provide three callback functions to the transaction module. The transaction module will call these three functions through the websocket. The parameters of the three functions are the same and the names are different.
#### Verifying a transaction
Cmd name: txValidator

It is used by the service module to validate the txData data. It can also verify whether the data such as coinData meets the business requirements. If the verification fails, the trading module will discard the transaction.
#### Save transaction business data
Cmd name: txCommit

It is used to save the business data in the transaction to the node local database, or to do the corresponding business logic processing. The transactions that arrive at this step are all consensus data.
#### Rollback transaction business data
Cmd name: txRollback

When the block is rolled back, the callback function will be triggered. The business module should clear the business data related to the transaction in the function, or perform corresponding reverse processing.
#### Callback function parameter list
| Parameter Name | Type | Parameter Description |
| --- | --- | --- |
| chainId | int | chain id (distinguishing data sources when nodes are running multiple chains) |
| txList | list | Trading List |
| blockHeader | object | block header |

#### Deserialization, [Common Agreement](https://github.com/nuls-io/nuls-v2-docs/blob/master/design-zh-EN/h.%E9%80%9A%E7%94%A8%E5%8D%8F%E8%AE%AE%E8%AE%BE%E8%AE%A1v1.3.md)
The data of the two parameters txList and blockHeader are transmitted in the form of hexadecimal data. First, the hexadecimal is converted into a byte array, and then deserialized into structured data according to different rules.
##### [Transaction](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/data/Transaction.java)
The txList stores a list of Transaction objects. Each item is a serialized Transaction object into a hexadecimal string. Deserializing txList first from [General Protocol](https://github.com/nuls-io/nuls-v2-docs/blob/master/design-zh-CHS/r.rpc-tool-websocket%E8%AE%BE%E8%AE%A1v1.3.md) takes the value of the txList parameter, which is a string array of json, and then iterates through the array to get the serialized value of a single Transaction object. Convert the serialized value to a byte array. Then take the corresponding data values ​​one by one from the byte array.
The rules for reading data in a byte array are as follows:
1. 2 bytes store unsigned 16-bit int to save the transaction type.
2. 4 bytes store unsigned 32-bit int to save transaction timestamp (January 1, 1970 to the current number of seconds)
3. Variable length type storage remark string, see [variable length type reading method](#Variable Length Type Storage Structure)
4. Become a type store txData string, business custom, but still need to be converted into a byte array.
5. The variable length type stores the coinData string, which is the hexadecimal string after the serialData object is serialized. See [CoinData Deserialization Method](#CoinData)
6. The variable length type stores the transaction signature string, which is a hexadecimal string serialized by the TransactionSignature object.

##### <span id="CoinData">[CoinData](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/data/CoinData.java)</span>
The CoinData object stores the relationship between the deposit and withdrawal of a transaction. A transaction withdrawal account and a deposit account support a many-to-many relationship. As long as the total withdrawal amount is greater than or equal to the total deposit amount plus the commission transaction, it can be established.
1. The [varint](https://learnmeabitcoin.com/glossary/varint) type stores the list of gold account information.
2. Store the gold account information list in order. The deposit account information is CoinFrom object. Note that the CoinFrom object is not processed in hexadecimal string.
3. [varint](https://learnmeabitcoin.com/glossary/varint) Type The number of lists of deposit account information.
4. Store the deposit account information list in order. The deposit account information is CoinTo object. Note that the CoinTo object is not processed in hexadecimal string.

##### <span id="CoinFrom">[CoinFrom](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/data/CoinFrom.java)</span>
1. Variable length type storage account address. [Address Serialization Code](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/basic/AddressTool.java )
2. 2 bytes store unsigned 16-bit int to save the asset chain id.
3. 2 bytes store unsigned 16-bit int to save the asset id.
4. 32 bytes store the numeric data of the BigInteger type to save the amount of gold assets.
5. Variable length type storage account nonce value.
6. 1 byte storage lock status (for consensus).

##### <span id="CoinTo">[CoinTo](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/data/CoinTo.java)</span>
1. Variable length type storage account address. [Address Serialization Code](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/basic/AddressTool.java )
2. 2 bytes store unsigned 16-bit int to save the asset chain id.
3. 2 bytes store unsigned 16-bit int to save the asset id.
4. 32 bytes store the numeric data of the BigInteger type to save the amount of gold assets.
5. 8 bytes store signed 64-bit long save lock time (time to lock assets).

##### <span id="TransactionSignature">[TransactionSignature](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/signture/TransactionSignature.java)</span>
There will be multiple signatures in front of the transaction, so the TransactionSignature stores the list of signature data. Multiple signatures are stored sequentially in a byte array. In the case of deserialization, it is rotated in turn.
1. 1 byte stores the public key length.
2. Public key data (length is obtained according to 1).
3. Variable length type stores signature data.

##### <span id="BlockHeader">[BlockHeader](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/data/BlockHeader.java)</span>
BlockHeader is a block header object, which mainly stores the hash value of the previous block, the root hash value of [merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), the block timestamp, the block height, and the block. Total number of transactions, block signatures, and extended data.
Serialization rules:
1. 32 bytes store the hash value of the previous block.
2. 32 bytes store the hash value of the merkle root.
3. 4 bytes store unsigned 32-bit int to save the block timestamp (from January 1, 1970 to the current number of seconds).
4. 4 bytes store unsigned 32-bit int to save block height.
5. 4 bytes store unsigned 32-bit int to save the total number of transactions in the current block.
6. Variable length type storage extension data.
7. The variable length type stores the transaction signature string, which is the hexadecimal string serialized by the BlockSignature object.

##### <span id="BlockSignature">[BlockSignature](https://github.com/nuls-io/nuls-v2/blob/master/common/nuls-base/src/main/java/io/nuls/base/signture/BlockSignature.java)</span>
1. 1 byte stores the public key length.
2. Public key data (length is obtained according to 1).
3. Variable length type stores signature data.

##### <span id="Variable Length Type Storage Structure"> Variable Length Type Storage Structure</span>
The variable length type consists of two parts. The first part is the length of the byte occupied by the varint type storage data, and the second part is the data part. The way to read the variable length type structure is to read the varint data first and read the corresponding length of the business data.
1. [varint](https://learnmeabitcoin.com/glossary/varint) type stores the length of the byte array.
2. Convert the business data into a byte array and store it.




### Module Template List
* [Java Module Development Template](https://github.com/nuls-io/nuls-module-template-java)
* [Blockchain Browser Template](https://github.com/nuls-io/nuls-module-explorer)

### <span id="doclist">Document List</span>
* [Java Module development template usage documentation](https://github.com/nuls-io/nuls-module-template-java)
* [Encrypted Mail Sample Module Design Document](https://github.com/CQBeer/nuls-chainbox/blob/master/example/%E6%A8%A1%E5%9D%97%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3.md)
* [Account Module RPC Interface Document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/account.md)
* [Book module RPC interface document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/ledger.md)
* [Transaction Module RPC Interface Document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/transaction.md)
* [Block module RPC interface document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/block.md)
* [Consensus Module RPC Interface Document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/consensus.md)
* [Network Module RPC Interface Document](https://github.com/nuls-io/nuls-chainbox/blob/master/document/network.md)
### Contribution

Contributions to NULS are welcomed! We sincerely invite developers who are experienced in the blockchain field to join the NULS technology community. [Details](https://nuls.community/d/9-recruitment-of-community-developers) To be a great community, Nuls needs to welcome developers from all walks of life, with different backgrounds, and with a wide range of experience.

### License

Nuls is released under the [MIT](http://opensource.org/licenses/MIT) license.
Modules added in the future may be release under different license, will specified in the module library path.

### Community

- [nuls.io](https://nuls.io/)
- [@twitter](https://twitter.com/nulsservice)
- [facebook](https://www.facebook.com/nulscommunity/)
- [YouTube channel](https://www.youtube.com/channel/UC8FkLeF4QW6Undm4B3InN1Q?view_as=subscriber)
- Telegram [NULS Community](https://t.me/Nulsio)
- Telegram [NULS 中文社区](https://t.me/Nulscn)

####
