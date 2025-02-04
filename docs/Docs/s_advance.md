# Advanced Description

## I. Contract transfers NULS out

**Transaction type txType = 18**

### 1. Transfer out implementation

In the contract SDK, there is a method in the Address object.

```java
/**
 * Contract transfers to this address
 *
 * @param value Transfer amount (how many Na)
 */
public native void transfer(BigInteger value);
```
When this method is executed within the smart contract method, the contract will transfer the corresponding asset to the specified Address, as shown in the following example:

```java
// Instantiate an Address object based on the account address
Address recipient = new Address("NULSd6HgkSpgKw3jqgbzNZ4FPodG4LEReq8cw");
// Transfer 18 nuls to this account address
recipient.transfer(BigInteger.valueOf(1800000000L));

```

### 2. Results query

If NULS is transferred from the contract address, it will be reflected in the execution result of the contract. Each contract transfer is displayed in the `transfers` array object in the result. The data displayed here is only the summary information of the contract transfer transaction.

Example: The `transfers` array object that intercepts the contract execution result is as follows

```json
"transfers": [
    {
        // Contract NULS assets are transferred out of trading hash
        "txHash": "4877f6a865dea5b4ac82a8370d73e62da15bc7acb2145a03822dddfdab329d2b",
        "from": "NULSd6Hgdf7bdag8wyRWjUuJgQ9pu46eoiV7d",
        "value": "1800000000",
        "outputs": [
            {
                "to": "NULSd6HgkSpgKw3jqgbzNZ4FPodG4LEReq8cw",
                "value": "1800000000"
            }
        ],
        / / Call the contract transaction hash
        "orginTxHash": "b5473eefecd1c70ac4276f70062a92bdbfe8f779cbe48de2d0315686cc7e6789"
    }
]
```

### 3.Complete transaction serialization data query

#### Trading background

<b style="color:red">Note: This type of transaction is not included in `block` because it is not broadcast on the node network.</b> 

Each node that receives the smart contract transaction executes the smart contract and saves the execution result on its own node. When all the contract transactions in a block are executed, a `stateRoot` will be generated. This `stateRoot` will be in the `block. `Broadcast.

#### inquiry mode

The complete transaction serialization data is also included in the contract execution result. The `contractTxList` array object in the result will contain the contract NULS asset transfer transaction generated after the execution of this contract.

The following is the result of the execution of the contract nuls asset transfer transaction

> via `RESTFUL` interface`/api/contract/result/{hash}` 
> 
> or 
> 
> Request data via `JSONRPC` interface `getContractTxResult `: 

```json
{
"jsonrpc":"2.0",
"method":"getContractTxResult",
"params":[chainId, hash],
"id":1234
}
```

<<<<<<< HEAD

In the following results, `contractTxList` will contain the transaction generated after the execution of this contract.

> Note: This structure is not limited to contract NULS asset transfer transactions, different contract transactions will be generated according to different business, such as contract consensus transaction --> [Smart Contract Consensus Transaction Description] (./consensusTransaction.html)
=======
In the following results, `contractTxList` is the transaction generated after the execution of this contract. Note: This structure is not limited to the contract NULS asset transfer transaction, and different contract NULS asset transfer transactions will be generated according to different business, such as contract consensus. Trading --> [Smart Contract Consensus Transaction Description](./consensusTransaction.html)
>>>>>>> febc3aaf7100522c6518e55bb40c840791032322

```json
{
    "success": true,
    "data": {
        "flag": true,
        "data": {
            "success": true,
            "errorMessage": null,
            "contractAddress": "NULSd6Hgdf7bdag8wyRWjUuJgQ9pu46eoiV7d",
            "result": "multyForAddress: 888634777633",
            "gasLimit": 200000,
            "gasUsed": 20038,
            "price": 25,
            "totalFee": "5100000",
            "txSizeFee": "100000",
            "actualContractFee": "500950",
            "refundFee": "4499050",
            "value": "0",
            "stackTrace": null,
            "transfers": [
                {
                    "txHash": "4877f6a865dea5b4ac82a8370d73e62da15bc7acb2145a03822dddfdab329d2b",
                    "from": "NULSd6Hgdf7bdag8wyRWjUuJgQ9pu46eoiV7d",
                    "value": "1800000000",
                    "outputs": [
                        {
                            "to": "NULSd6HgkSpgKw3jqgbzNZ4FPodG4LEReq8cw",
                            "value": "1800000000"
                        }
                    ],
                    "orginTxHash": "b5473eefecd1c70ac4276f70062a92bdbfe8f779cbe48de2d0315686cc7e6789"
                }
            ],
            "events": [],
            "tokenTransfers": [],
            "invokeRegisterCmds": [],
            "contractTxList": [
                "12002fbb225d0037b5473eefecd1c70ac4276f70062a92bdbfe8f779cbe48de2d0315686cc7e678902000253472f4702eb83b71871a4c4e0c71526bb86b8afd0011702000253472f4702eb83b71871a4c4e0c71526bb86b8af0200010000c2eb0b0000000000000000000000000000000000000000000000000000000008000000000000000000021702000194f6239c075d184e265eaea97a67eeced51725160200010000e1f50500000000000000000000000000000000000000000000000000000000000000000000000017020001ce8ffa95606f0bfd2778cff2eff8fe8999e20c440200010000e1f50500000000000000000000000000000000000000000000000000000000000000000000000000"
            ],
            "remark": "call"
        }
    }
}
```


## II. Smart Contract Fee

### 1. Who pays for the smart contract fee?

When `Create Smart Contracts`, `Call Smart Contracts`, `Delete Smart Contracts`, the handling fee is paid by the address at which the transaction is initiated, and the contract address itself does not pay the handling fee.

### 2. How to charge for smart contract fees, how to charge? How much does the interface caller pay? Who received these fees?

In the main chain, there are three more types of transactions, `Create Smart Contracts`, `Call Smart Contracts`, `Delete Smart Contracts`.

The difference between the three transactions and other transactions such as `transfer" lies in the execution of one smart contract, so the execution of smart contracts is also one of the charging standards. 

* Smart contract charging calculation method

```java
Public static final int COMPARISON = 1;//Compare bytecode
Public static final int CONSTANT = 1;//simple numeric type bytecode
Public static final int LDC = 1; / / numeric constant, string constant (length * LDC)
Public static final int CONTROL = 5;//control bytecode
Public static final int TABLESWITCH = 2; // switch bytecode (size * TABLESWITCH)
Public static final int LOOKUPSWITCH = 2; // switch bytecode (size * LOOKUPSWITCH)
Public static final int CONVERSION = 1;//Value conversion
Public static final int EXTENDED = 1;//null
Public static final int MULTIANEWARRAY = 1;//Multidimensional array (size * MULTIANEWARRAY)
Public static final int LOAD = 1; / / send local variables to the top of the stack
Public static final int ARRAYLOAD = 5;//Send an item of the array to the top of the stack
Public static final int MATH = 1;//Mathematical operations and shift operations
Public static final int REFERENCE = 10;//object related operations
Public static final int NEWARRAY = 1;//One-dimensional array (size * NEWARRAY)
Public static final int STACK = 2; / / stack operation
Public static final int STORE = 1;//Save the value of the top of the stack to a local variable
Public static final int ARRAYSTORE = 5; / / store the value of the stack item into the array
Public static final int TRANSFER = 1000;//transfer transaction

```
    
* One smart contract total handling fee

    The total handling fee for a contract transaction consists of three parts
    - The first part is the transaction fee generated by the transaction size, calculated according to the byte size -> 0.001nuls/kb, which is 0.001 nuls per 1000 bytes, the transaction size is less than 1000 bytes, and the charge is 0.001 nuls.
    
    - The second part is the GAS*Price consumed by the contract execution. Price is the unit price, which means how much Na is for each Gas, Na is the smallest unit of NULS, 1Nuls is 100 million Na.
    > For example, if a contract execution consumes 20,000Gas and the set price is 30Na/Gas, then the Na consumed this time is `20000 * 30 = 600000`, which is 0.006NULS
    
    - The third part is that the GasLimit set by the caller is not consumed by the current contract execution, and the remaining Gas will be returned by the contract GAS return transaction.
    > For example, the last chestnut, the GasLimit set for the contract is 30000Gas, and the contract execution consumes 20,000Gas, then the remaining 10000Gas, the 10000Gas converted to Na is `10000 * 30 = 300000`, which is 0.003NULS, then this 0.003NULS will return the contract caller in the contract GAS return transaction of the current packaged block
    
* How much does the contract caller pay?

    In the contract transaction, the contract caller pays the first, second and third parts. In fact, the contract caller pays the first and second parts, because the third part will be returned to the contract call in the contract gas return transaction of the current packaged block. By

* Who received these fees?
    
    The block packer receives the first and second part of the fee, and the contract caller receives the third part of the fee.

## III. Triggering the scene of the payable method

In `vm-sdk`, there is such a description for `Contract#payable`

```java
package io.nuls.contract.sdk;

/**
 * Contract interface, contract class implements this interface
 */
public interface Contract {

    /**
     * Directly transfer to the contract, this method will be triggered, the default is to do nothing, you can override this method.
     * Prerequisite: This method needs to be overloaded and marked with the `@Payable` annotation.
     */
    default void _payable() {
    }

    /**
     * 1. This method is triggered when the consensus node reward address is the contract address. The parameter is the block reward address detail eg. [[address, amount], [address, amount], ...]
     * 2. This method is triggered when the delegate node address is a contract address. The parameter is the contract address and the bonus amount eg. [[address, amount]]
     * Prerequisite: This method needs to be overloaded and marked with the `@Payable` annotation.
     */
    default void _payable(String[][] args) {
    }
}

```

`mm-sdk``maven` depends on the following

```xml
<dependency>
    <groupId>io.nuls.sdk</groupId>
    <artifactId>sdk-contract-vm</artifactId>
    <version>LATEST</version>
</dependency>
```

### 1. the official wallet transfer function, triggered when the account address is transferred to the contract address

Trigger `payable()` no argument method

**System underlying implementation principle:**

Assembled into a call contract transaction, the default call to the contract's `_payable()` method

### 2. the consensus node reward address is the contract address, triggered when the current node is out of the block.

Trigger `_payable(String[][] args)` has a parameter method, the parameter is the current block _** all **_ reward address details eg. [[address, amount], [address, amount], ...]

**System underlying implementation principle:**

When the consensus module determines that the reward address in the CoinBase transaction has a contract address, the `_payable(String[][] args)` method of the contract is invoked, and the corresponding revenue amount is transferred to the contract address.

### 3. The entrusted address is the contract address, which is triggered when there is a current contract address in the block reward.

Trigger `_payable(String[][] args)` has a parameter method, the parameter _** is and is only **_ current contract address and bonus amount (only this one element) eg. [[address, amount]]

**System underlying implementation principle:**

When the consensus module determines that the reward address in the CoinBase transaction has a contract address, the `_payable(String[][] args)` method of the contract is invoked, and the corresponding revenue amount is transferred to the contract address.

### 4. Triggered when the user directly calls the `_payable()` method of the contract

### Note: The `_payable(String[][] args)` method is a system call method that the user cannot call.


## IV. Contract Call External Command Description

```java
public class Utils {
    /**
     * Commands that call other modules on the chain
     *
     * @param cmdName command name
     * @param args command parameter
     * @return command returns a value (depending on the return type of the registration command, it can return a string, a string array, a two-dimensional array of strings)
     */
    public static native Object invokeExternalCmd(String cmdName, String[] args);
}
```

### 1. The function of generating contract consensus transactions

#### 1.1 Creating a consensus node

- Command name

    **`cs_createContractAgent `**
    
- Transaction Type 
  
    **txType = `20`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |packingAddress |Package Address|String |AddressTool.getStringAddressByBytes(byte[])|Slightly|
    |deposit |Margin |String |BigInteger - toString|Slightly|
    |commissionRate | Commission Ratio | String | Byte - toString | Range [10,100]|

- Return value description
  
    > Return value type is **`String`** string type
    
    |Description|String Conversion Logic|
    |:---------:|:---------:|
    |transaction hash | slightly |

- example

    ```java
    // String packingAddress, BigInteger depositNa, String commissionRate
    String[] args = new String[]{packingAddress, depositNa.toString(), commissionRate};
    String txHash = (String) Utils.invokeExternalCmd("cs_createContractAgent", args);        
    ```

#### 1.2 Contract delegation consensus node

- Command name

    **`cs_contractDeposit `**

- Transaction Type 
  
    **txType = `21`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |agentHash |Create contract consensus node transaction hash |String |略|略|
    |deposit |trust amount|String |BigInteger - toString|略|

- Return value description
  
    > Return value type is **`String`** string type
    
    |Description|String Conversion Logic|
    |:---------:|:---------:|
    |transaction hash | slightly |
    
- example

    ```java
    // String agentHash, BigInteger depositNa
    String[] args = new String[]{agentHash, depositNa.toString()};
    String txHash = (String) Utils.invokeExternalCmd("cs_contractDeposit", args);       
    ```

#### 1.3 Contract Exit Consensus Node

- Command name

    **`cs_contractWithdraw`**

- Transaction Type 
  
    **txType = `22`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |joinAgentHash | Trading when joining consensus = hash | slightly | slightly |

- Return value description
  
    > Return value type is **`String`** string type
    
    |Description|String Conversion Logic|
    |:---------:|:---------:|
    |transaction hash | slightly |

- example

    ```java
    // String joinAgentHash
    String[] args = new String[]{joinAgentHash};
    String txHash = (String) Utils.invokeExternalCmd("cs_contractWithdraw", args);       
    ```    
    
#### 1.4 Contract cancellation consensus node

- Command name

    **`cs_stopContractAgent `**

- Transaction Type 
  
    **txType = `23`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    No parameters

- Return value description
  
    > Return value type is **`String`** string type
    
    |Description|String Conversion Logic|
    |:---------:|:---------:|
    |transaction hash | slightly |

- example

    ```java
    String txHash = (String) Utils.invokeExternalCmd("cs_stopContractAgent", null);      
    ```
    
### 2. Query consensus data

#### 2.1 According to _` join the consensus transaction hash`_ query delegate consensus information

- Command name

    **`cs_getContractDepositInfo `**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |joinAgentHash | Trading when joining consensus = hash | slightly | slightly |

- Return value description
  
    > Return value type is **`String[]`** string array type
    
    |index |Description|String Conversion Logic|
    |:---------:|:---------:|:---------:|
    |0 | agentHash | Slightly |
    |1    | agentAddress     |AddressTool.getStringAddressByBytes(byte[])|
    |2    | joinAddress     |AddressTool.getStringAddressByBytes(byte[])|
    |3    | deposit     |BigInteger - toString|
    |4    | time     |Long - toString|
    |5    | blockHeight     |Long - toString|
    |6    | delHeight     |Long - toString|
    |7 | status(0-to-consult 1 - consensus) | Integer - toString|

- example

    ```java
    // String joinAgentHash
    String[] args = new String[]{joinAgentHash};
    String[] contractDepositInfo = (String[]) Utils.invokeExternalCmd("cs_getContractDepositInfo", args);  
    
    // Commission amount
    BigInteger deposit = new BigInteger(contractDepositInfo[3]);
    // 0 - to be consensus 1 - consensus
    String status = contractDepositInfo[7];     
    ```

#### 2.2 Creating a node's transaction hash`_ query node information according to _`

- Command name

    **`cs_getContractAgentInfo `**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |agentHash |Create contract consensus node transaction hash |String |略|略|

- Return value description
  
    > Return value type is **`String[]`** string array type
    
    |index |Description|Chinese description|String conversion logic|
    |:---------:|:---------:|:---------:|:---------:|
    |0 | agentAddress |Create node address|AddressTool.getStringAddressByBytes(byte[])|
    |1 | packingAddress |Package Address|AddressTool.getStringAddressByBytes(byte[])|
    |2 | rewardAddress |Reward Address|AddressTool.getStringAddressByBytes(byte[])|
    |3 | deposit | Margin | BigInteger - toString|
    |4 | totalDeposit | Total Delegate Amount | BigInteger - toString|
    |5 | commissionRate | Commission Ratio | Integer - toString|
    |6 | time |Create Node Time|Long - toString|
    |7 | blockHeight |Create node height |Long - toString|
    |8 | delHeight | The height of the logout node | Long - toString|
    |9 | status | Status (0 - Consensus 1 - Consensus) | Integer - toString|

- example

    ```java
    // String agentHash
    String[] args = new String[]{agentHash};
    String[] contractAgentInfo = (String[]) Utils.invokeExternalCmd("cs_getContractAgentInfo", args);  
    
    // The contract node has commissioned the amount
    BigInteger totalDepositOfContractAgent = new BigInteger(contractAgentInfo[4]);
    // 0 - to be consensus 1 - consensus
    String statusOfContractAgent = contractAgentInfo[9];     
    ```
    
## V. Execution result description

### 1. Description of execution results

```json
{
    "success": true, //Contract execution is successful,
    "errorMessage": null, //failure reason - string, eg. not enough gas,
    "contractAddress": "tNULSeBaN1rhd9k9eqNkvwC9HXBWLQ79dRuy81",
    "result": "multyForAddress: 888634777633",
    "gasLimit": 200000,
    "gasUsed": 20038,
    "price": 25,
    "totalFee": "5100000",
    "txSizeFee": "100000",
    "actualContractFee": "500950",
    "refundFee": "4499050",
    "value": 10000000000, //The NULS that the contract caller transferred to the contract address, 0 if there is no such service.
    "stackTrace": null, //failed exception stack information - string, execution failure does not necessarily have ,
    "transfers": [
        //This refers to the transaction information of the contract address transferred out of the main network currency (NULS), which has nothing to do with the token, has nothing to do with the token, and has nothing to do with the token. Under normal circumstances, the contract transaction of the token transfer will not have such a transaction. The following is an example. 
        {
            "txHash": "4877f6a865dea5b4ac82a8370d73e62da15bc7acb2145a03822dddfdab329d2b",
            "from": "tNULSeBaN1rhd9k9eqNkvwC9HXBWLQ79dRuy81",
            "value": "200000000",
            "outputs": [
                {
                    "to": "tNULSeBaMp9wC9PcWEcfesY7YmWrPfeQzkN1xL",
                    "value": "100000000"
                },
                {
                    "to": "tNULSeBaMshNPEnuqiDhMdSA4iNs6LMgjY6tcL",
                    "value": "100000000"
                }
            ],
            "orginTxHash": "b5473eefecd1c70ac4276f70062a92bdbfe8f779cbe48de2d0315686cc7e6789"
        }
    ],
    "events": [
        //Event information sent within the contract - According to the token contract standard, a contract token transfer will send a corresponding event message called TransferEvent, which supports multiple token transfer within one contract call transaction.
        //The event content structure returned is the json structure.
        "{\"contractAddress\":\"TTb1LZLo6izPGmXa9dGPmb5D2vpLpNqA\",\"blockNumber\":1343847,\"event\":\"TransferEvent\",\"payload\":{\"from\":\"TTasNs8MGGGaFT9hd9DLmkammYYv69vs\",\"to\":\"TTau7kAxyhc4yMomVJ2QkMVECKKZK1uG\",\"value\":\"1000\"}}"
    ],
    "tokenTransfers": [
        //Data processing for the above token transfer event (TransferEvent), supplementing the basic information of the contract in which the token transfer occurred - name, symbol, decimals
        //Note 1: The value here is the decimal value storage value after the conversion of the contract token value, with the Ethereum token method.
        //Note 2: The contract address of the generated token transfer is not necessarily the contract currently called, so there is a contractAddress attribute in this data structure, which is not a redundant field.
        {
            "contractAddress": "TTb1LZLo6izPGmXa9dGPmb5D2vpLpNqA",
            "from": "TTasNs8MGGGaFT9hd9DLmkammYYv69vs",
            "to": "TTau7kAxyhc4yMomVJ2QkMVECKKZK1uG",
            "value": "1000",
            "name": "a",
            "symbol": "a",
            "decimals": 8
        }
    ],
    "invokeRegisterCmds": [
        //Contract to create consensus, call external command record
        {
            "cmdName": "cs_createContractAgent",
            "args": {
                "contractBalance": "2030000000000",
                "commissionRate": "100",
                "chainId": 2,
                "deposit": "2000000000000",
                "contractAddress": "tNULSeBaMzZedU4D3xym1JcyNa5sqtuFku8AKm",
                "contractNonce": "0000000000000000",
                "blockTime": 1562564381,
                "packingAddress": "tNULSeBaMtEPLXxUgyfnBt9bpb5Xv84dyJV98p",
                "contractSender": "tNULSeBaMvEtDfvZuukDf2mVyfGo3DdiN8KLRG"
            },
            "cmdRegisterMode": "NEW_TX",
            "newTxHash": "a8eae11b52990e39c9d3233ba1d2c8827336d261c0f14aca43dd4f06435dfaba"
        }
    ],
    "contractTxList": [
        //The transaction generated after the current execution of the smart contract
        "12002fbb225d0037b5473eefecd1c70ac4276f70062a92bdbfe8f779cbe48de2d0315686cc7e678902000253472f4702eb83b71871a4c4e0c71526bb86b8afd0011702000253472f4702eb83b71871a4c4e0c71526bb86b8af0200010000c2eb0b0000000000000000000000000000000000000000000000000000000008000000000000000000021702000194f6239c075d184e265eaea97a67eeced51725160200010000e1f50500000000000000000000000000000000000000000000000000000000000000000000000017020001ce8ffa95606f0bfd2778cff2eff8fe8999e20c440200010000e1f50500000000000000000000000000000000000000000000000000000000000000000000000000",
        "1400bf6b285d006600204aa9d1010000000000000000000000000000000000000000000000000000020002f246b18e8c697f00ed9bd22696998e469d3f824b020001d7424d91c83566eb94233b5416f2aa77709c03e1020002f246b18e8c697f00ed9bd22696998e469d3f824b648c0117020002f246b18e8c697f00ed9bd22696998e469d3f824b0200010000204aa9d1010000000000000000000000000000000000000000000000000000080000000000000000000117020002f246b18e8c697f00ed9bd22696998e469d3f824b0200010000204aa9d1010000000000000000000000000000000000000000000000000000ffffffffffffffff00"
    ],
    "remark": "call"
}
```

## VI. Contract Consensus Transaction Description

The Consensus Module provides four contract-related consensus transactions. When the module starts, it registers with the contract module to create four contract consensus transactions, so that the contract module can be called, so that it can create and cancel the consensus node, delegate and cancel the delegation consensus. node

### 1. Generate contract consensus transactions

#### 1.1 Creating a consensus node

- Original parameter list 

    > **`Create consensus node address`, `consensus reward address` fixed to `current contract address`**
    
    |Name|Description|Type|Optional|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |packingAddress |package address|byte[] |y|method call required |
    |deposit | margin | BigInteger | yes | method call required |
    |commissionRate | commission ratio | byte | yes | method call required - range [10,100]|
    |agentAddress |Create Node Address|byte[] |**No** |The underlying default is **`current contract address`**|
    |rewardAddress |Reward Address|byte[] |**No** |The underlying default is **`current contract address`**|

- registration message

    > **Consensus module registration command information for the contract consensus node registered with the smart contract module**

    |Module Code|cmd Name|Registration Type|Parameter Name List|Return Value Type|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|:---------:|
    |consensus | cs_createContractAgent |0(NEW_TX)|List.of("packingAddress","deposit","commissionRate")|1(STRING_ARRAY) |Create Consensus Node|
    
- Transaction Type 
  
    **txType = `20`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    >
    > When calling this method, the consensus module will receive the following parameters for this command
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |packingAddress |Package Address|String |AddressTool.getStringAddressByBytes(byte[])|Slightly|
    |deposit |Margin |String |BigInteger - toString|Slightly|
    |commissionRate | Commission Ratio | String | Byte - toString | Range [10,100]|
    |chainId |current chain ID |int |-|lower layer provides parameters|
    |contractAddress |Contract Address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractSender |Contract caller address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractBalance | Current balance of contract address|String |BigInteger -toString|Bottom providing parameters - use this value to check balance when generating transactions |
    |contractNonce | The current nonce value of the contract address|String |RPCUtil.encode(byte[])|The underlying provides the parameters - use this nonce to assemble the transaction when generating the transaction|
    |blockTime | currently packaged block time|long |-|lower layer provides parameters - use this as transaction time when generating transactions |

- Return value description

    - The smart contract virtual machine receives the **`String[]`** string array type
    
        |index |Description|String Conversion Logic|
        |:---------:|:---------:|:---------:|
        |0 | Trading hash | Slightly |
        |1 |Transaction Serialization String|RPCUtil.encode(byte[])|
    
    - The caller receives a **`String`** string type

        |Description|String Conversion Logic|
        |:---------:|:---------:|
        |transaction hash | slightly |

#### 1.2 Contract delegation consensus node

- original parameters 

    > **`Delegation Address` is fixed to `current contract address`**
    
    |Name|Description|Type|Optional|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |agentHash |Create contract consensus node transaction hash |String |Yes |Method call required |
    |deposit |dealer amount|BigInteger |yes|method call required|
    |address |trust address|byte[] |**no** |The underlying default is **`current contract address`**|

- registration message

    > **Consensus module registration command information for the contract creation consensus node registered with the smart contract module**
    
    |Module Code|cmd Name|Registration Type|Parameter Name List|Return Value Type|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|:---------:|
    |consensus | cs_contractDeposit |0(NEW_TX)|List.of("agentHash","deposit")|1(STRING_ARRAY) |Delegate Consensus Node|

- Transaction Type 
  
    **txType = `21`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    >
    > When calling this method, the consensus module will receive the following parameters for this command
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |agentHash |Create contract consensus node transaction hash |String |略|略|
    |deposit |trust amount|String |BigInteger - toString|略|
    |chainId |current chain ID |int |-|lower layer provides parameters|
    |contractAddress |Contract Address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractSender |Contract caller address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractBalance | Current balance of contract address|String |BigInteger - toString|Bottom providing parameters - use this value to check balance when generating transactions |
    |contractNonce | The current nonce value of the contract address|String |RPCUtil.encode(byte[])|The underlying provides the parameters - use this nonce to assemble the transaction when generating the transaction|
    |blockTime | currently packaged block time|long |-|lower layer provides parameters - use this as transaction time when generating transactions |

- Return value description
  
    - The smart contract virtual machine receives the **`String[]`** string array type
    
        |index |Description|String Conversion Logic|
        |:---------:|:---------:|:---------:|
        |0 | Trading hash | Slightly |
        |1 |Transaction Serialization String|RPCUtil.encode(byte[])|
    
    - The caller receives a **`String`** string type

        |Description|String Conversion Logic|
        |:---------:|:---------:|
        |transaction hash | slightly |

#### 1.3 Contract Exit Consensus Node

- original parameters 

    > **`Create node's address` is fixed to `current contract address`**
    
    |Name|Description|Type|Optional|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |joinAgentHash |Transaction hash when joining consensus |String |Yes |Method call required |
    |agentAddress |Create node address|byte[] |**No** |The underlying default is **`current contract address`**|

- registration message

    > **Consensus module registration contract information registered with the smart contract module to exit the consensus node**
    
    |Module Code|cmd Name|Registration Type|Parameter Name List|Return Value Type|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|:---------:|
    |consensus | cs_contractWithdraw |0(NEW_TX)|List.of("joinAgentHash")|1(STRING_ARRAY) |Exit the delegate consensus node|

- Transaction Type 
  
    **txType = `22`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    >
    > When calling this method, the consensus module will receive the following parameters for this command
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |joinAgentHash | Trading when joining consensus = hash | slightly | slightly |
    |chainId |current chain ID |int |-|lower layer provides parameters|
    |contractAddress |Contract Address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractSender |Contract caller address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |blockTime | currently packaged block time|long |-|lower layer provides parameters - use this as transaction time when generating transactions |

- Return value description
  
    - The smart contract virtual machine receives the **`String[]`** string array type
    
        |index |Description|String Conversion Logic|
        |:---------:|:---------:|:---------:|
        |0 | Trading hash | Slightly |
        |1 |Transaction Serialization String|RPCUtil.encode(byte[])|
    
    - The caller receives a **`String`** string type

        |Description|String Conversion Logic|
        |:---------:|:---------:|
        |transaction hash | slightly |
    
    
#### 1.4 Contract cancellation consensus node

- original parameters 
  
    > **`Create node's address` is fixed to `current contract address`**
    
    |Name|Description|Type|Optional|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |agentAddress |Create node address|byte[] |**No** |The underlying default is **`current contract address`**|

- registration message

    > **Consultation module registration contract cancellation agreement node transaction command information registered with the smart contract module**
    
    |Module Code|cmd Name|Registration Type|Parameter Name List|Return Value Type|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|:---------:|
    |consensus |cs_stopContractAgent |0(NEW_TX)|`empty list`|1(STRING_ARRAY) |Logout Consensus Node|

- Transaction Type 
  
    **txType = `23`**
    
- Method parameter specification
  
    > When the contract calls this method, the parameter types passed are `String` type
    >
    > When calling this method, the consensus module will receive the following parameters for this command
    
    |Name|Description|Type|Contract Conversion `String` Logic|Remarks|
    |:---------:|:---------:|:---------:|:---------:|:---------:|
    |chainId |current chain ID |int |-|lower layer provides parameters|
    |contractAddress |Contract Address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |contractSender |Contract caller address|String |AddressTool.getStringAddressByBytes(byte[])|The underlying provides parameters|
    |blockTime | currently packaged block time|long |-|lower layer provides parameters - use this as transaction time when generating transactions |

- Return value description
  
    - The smart contract virtual machine receives the **`String[]`** string array type
    
        |index |Description|String Conversion Logic|
        |:---------:|:---------:|:---------:|
        |0 | Trading hash | Slightly |
        |1 |Transaction Serialization String|RPCUtil.encode(byte[])|
    
    - The caller receives a **`String`** string type

        |Description|String Conversion Logic|
        |:---------:|:---------:|
        |transaction hash | slightly |
    

### 2. How to obtain contract consensus transaction data

Contract consensus transactions are not signed, and such transactions are generated when each node verifies the block. In addition, such transactions are not in the scope of broadcast data, and need to be retrieved from another interface. Please refer to the following interface for detailed data** To get the _serialized string_data of the transaction.

The following is the result of the execution of the contract internal transfer transaction.

> via `RESTFUL` interface`/api/contract/result/{hash}` 
> 
> or 
> 
> Request data via `JSONRPC` interface `getContractTxResult `: 

```json
{
"jsonrpc":"2.0",
"method":"getContractTxResult",
"params":[chainId, hash],
"id":1234
}
```
>
> Get the execution result of the contract

In the following results, `contractTxList` is the transaction generated after the execution of this contract. Note: This structure is not limited to contract consensus transactions, and different contract internal transactions will be generated according to different business, such as contract NULS asset transfer transaction-- > [Smart Contract NULS Asset Transfer Transaction Instructions](./assetsOff.html)

```json
{
    "success": true,
    "data": {
        "flag": true,
        "data": {
            "success" : true,
            "errorMessage" : null,
            "contractAddress" : "tNULSeBaNBdmatSnyhbgwm2gdkJHzKtpR2nGbQ",
            "result" : null,
            "gasLimit" : 200000,
            "gasUsed" : 13766,
            "price" : 25,
            "totalFee" : "5100000",
            "txSizeFee" : "100000",
            "actualContractFee" : "344150",
            "refundFee" : "4655850",
            "value" : "2000000000000",
            "stackTrace" : null,
            "transfers" : [ ],
            "events" : [ ],
            "tokenTransfers" : [ ],
            "invokeRegisterCmds" : [ {
              "cmdName" : "cs_createContractAgent",
              "args" : {
                "contractBalance" : "2000000000000",
                "commissionRate" : "100",
                "chainId" : 2,
                "deposit" : "2000000000000",
                "contractAddress" : "tNULSeBaNBdmatSnyhbgwm2gdkJHzKtpR2nGbQ",
                "contractNonce" : "0000000000000000",
                "blockTime" : 1562930111,
                "packingAddress" : "tNULSeBaMtEPLXxUgyfnBt9bpb5Xv84dyJV98p",
                "contractSender" : "tNULSeBaMvEtDfvZuukDf2mVyfGo3DdiN8KLRG"
              },
              "cmdRegisterMode" : "NEW_TX",
              "newTxHash" : "6278df86951ef140b98a57a27ff764453ae437d8aea2888ded886eb70ee11348"
            } ],
            "contractTxList" : [ 
            "1400bf6b285d006600204aa9d1010000000000000000000000000000000000000000000000000000020002f246b18e8c697f00ed9bd22696998e469d3f824b020001d7424d91c83566eb94233b5416f2aa77709c03e1020002f246b18e8c697f00ed9bd22696998e469d3f824b648c0117020002f246b18e8c697f00ed9bd22696998e469d3f824b0200010000204aa9d1010000000000000000000000000000000000000000000000000000080000000000000000000117020002f246b18e8c697f00ed9bd22696998e469d3f824b0200010000204aa9d1010000000000000000000000000000000000000000000000000000ffffffffffffffff00" 
            ],
            "remark" : "call"
        }
    }
}
```

## VII. Transaction instructions for transferring contracts to nuls assets

Ordinary account address, to the contract into the nuls, must be achieved through the `call contract' transaction

** Call contract parameter list**
 
| Parameter Name | Parameter Type | Parameter Description | Required |
| --------------- |:----------:| ---------------------------------------- |:----:|
| chainId | int | chain id | yes |
| sender | string | Transaction Creator Account Address | Yes |
| password | string | caller account password | yes |
| value | biginteger | The amount of the primary network asset that the caller transferred to the contract address. If there is no such service, fill BigInteger.ZERO | Yes |
| gasLimit | long | GAS Limit | Yes |
| price | long | GAS unit price | Yes |
| contractAddress | string | contract address | yes |
| methodName | string | contract method | yes |
| methodDesc | string | Contract method description, if the method in the contract is not overloaded, this parameter can be empty | No |
| args | object[] | List of parameters | No |
| remark | string | Transaction Notes | No|

### 1. Transfer to implementation

You can transfer the contract to NULS by filling in the corresponding amount in the `value` of the calling contract parameter.

Example of nuls-api restful request method:

```json
{
  "sender" : "tNULSeBaMvEtDfvZuukDf2mVyfGo3DdiN8KLRG",
  "gasLimit" : 20000,
  "price" : 25,
  "password" : "nuls123456",
  "remark" : null,
  "contractAddress" : "tNULSeBaMx7J2im9edmmyZofHoTWW6nCTbvy3K",
  // Fill in the NULS to be transferred here, the unit is Na
  "value" : 3600000000,
  "methodName" : "transferToContractTest",
  "methodDesc" : null,
  "args" : [ "method parameter"]
}
```
In the above example, the general account `tNULSeBaMvEtDfvZuukDf2mVyfGo3DdiN8KLRG` transferred 36 NULS to the contract address `tNULSeBaMx7J2im9edmmyZofHoTWW6nCTbvy3K`.

<b style="color:red">Note: The contract method invoked must be marked with a Payable annotation, otherwise the system will directly reject the transaction.</b>

Contract code - method implementation is as follows

```java
/**
 * Mark @Payable method to pass NULS amount when calling
 */
@Payable
public void transferToContractTest(String storedData) {
    // The NULS that the caller transferred to the contract, the unit is Na
    BigInteger value = Msg.value();
}
```

The contract uses the `Msg.value()` to get the NULS that the caller transferred to the contract. The unit is Na, as in the above code.

## VIII、how to debug smart contracts

The contract SDK provides a DebugEvent. If the contract fails to execute, you can see the data of this event. Use it to debug the contract.

### 1. Debugging way

When writing a contract, use the `emit(new DebugEvent("name", "desc"))` event to send it. After the contract is released, the contract method is called. 

If the execution is successful, the debugEvent data will be displayed in the contract execution result. 

If the execution fails, the debug event data will be displayed in the returned error data.

<b style="color:red">Note: Each time you call a contract, you can display up to 10 DebugEvents, and the excess will be ignored by the Smart Contract VM. </b>

### 2. Contract code example

```java
/**
 * Example of successful call (test network)
 */
public Object clinitTest() {
    Address temp = new Address("tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD");
    String asd = "tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD";
    Utils.emit(new DebugEvent("clinitTest log", "asd is " + asd));
    int qwe = 123;
    Utils.emit(new DebugEvent("clinitTest log 1", "temp is " + temp));
    return temp;
}

/**
 * Example of call failure (test net)
 */
public Object clinitTestRevert() {
    Address temp = new Address("tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD");
    String asd = "tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD";
    Utils.emit(new DebugEvent("clinitTest log", "asd is " + asd));
    int qwe = 123;
    Utils.emit(new DebugEvent("clinitTest log 1", "temp is " + temp));
    // failed
    Utils.revert("revert");
    return temp;
}
```

### 3.  Example of execution failure
       
When the execution fails, the debugEvent data is displayed in the error message.

As in the above contract, we use `revert("revert")` in the `clinitTestRevert` method to make this call fail to execute, and return the DebugEvent event data if the simulation fails.

#### 3.1 Error data returned by page call failure

![](../zh/Docs/debugcontract/debugcontract.png)

#### 3.2 `NULS-API RESTFUL` mode call failed error data returned

`http://beta.api.nuls.io/api/contract/call`

```json
{
    "sender" : "tNULSeBaMiKUm9zpU1bhXeaaZt2AdLgPTs3T28",
    "gasLimit" : 200000,
    "price" : 25,
    "password" : "abc123456",
    "remark" : "remark-restful-call",
    "contractAddress" : "tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR",
    "value" : 0,
    "methodName" : "clinitTestRevert",
    "methodDesc" : null,
    "args" : null
}
```

```json
{
    "success": false,
    "data": {
        "code": "err_0014",
        "msg": "contract error - revert, debugEvents: [{\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":201112,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log\",\"desc\":\"asd is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}, {\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":201112,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log 1\",\"desc\":\"temp is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}]"
    }
}
```

#### 3.3 `NULS-API JSONRPC` mode call failed error data returned

`http://beta.api.nuls.io/jsonrpc`

```json
{
    "jsonrpc":"2.0",
    "method":"contractCall",
    "params":[2,
                "tNULSeBaMiKUm9zpU1bhXeaaZt2AdLgPTs3T28",
                "abc123456",
                0,
                200000,
                25,
                "tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR",
                "clinitTestRevert",
                null,
                [],
                "remark-jsonrpc-call"],
    "id":1234
}
```

```json
{
    "jsonrpc": "2.0",
    "id": "1234",
    "error": {
        "code": "err_0014",
        "message": "contract error - revert, debugEvents: [{\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":194030,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log\",\"desc\":\"asd is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}, {\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":194030,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log 1\",\"desc\":\"temp is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}]",
        "data": null
    }
}
```

### 4. Example of successful execution

In the case of successful execution, the data of the debugEvent will also be displayed in the contract execution result.

http://beta.api.nuls.io/api/contract/result/1aaab3e9453468dd1a4569d0d6d9887b636ee2438671746332125ac6e44ae409

```json
{
    "success": true,
    "data": {
        "flag": true,
        "data": {
            "success": true,
            "errorMessage": null,
            "contractAddress": "tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR",
            "result": "tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD",
            "gasLimit": 6081,
            "gasUsed": 4054,
            "price": 25,
            "totalFee": "252025",
            "txSizeFee": "100000",
            "actualContractFee": "101350",
            "refundFee": "50675",
            "value": "0",
            "stackTrace": null,
            "transfers": [],
            "events": [
                "{\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":194018,\"event\":\"TempEvent\",\"payload\":{\"temp\":\"123\"}}"
            ],
            "debugEvents": [
                "{\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":194018,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log\",\"desc\":\"asd is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}",
                "{\"contractAddress\":\"tNULSeBaNA416GsttuWmWJwHrgJ8KfWzVw4LQR\",\"blockNumber\":194018,\"event\":\"DebugEvent\",\"payload\":{\"name\":\"clinitTest log 1\",\"desc\":\"temp is tNULSeBaMnrs6JKrCy6TQdzYJZkMZJDng7QAsD\"}}"
            ],
            "tokenTransfers": [],
            "invokeRegisterCmds": [],
            "contractTxList": [],
            "remark": "call"
        }
    }
}
```
