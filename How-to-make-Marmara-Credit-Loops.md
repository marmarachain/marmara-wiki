The current Marmara Credit loops currently work based on Protocol 1 which is in 100% collateralization mode. 100 % collateralization is made by issuer on behalf of both himself/herself and holder. Both issuer and holder have the 3x staking chance to get blockchain rewards. Issuer has the 3x staking chance until maturity date of credit whereas holder has the 3x staking chance until he/she endorses/transfers the credit to a new holder who will continue staking with the issuer. 
The Credit loops can be made using only activated coins.
### Terminology
**Issuer:** The person who first creates a credit in a credit loop. It is the person who forms the first node in the credit loop. The credit may be collateralized 100% or with even zero-collateralization until maturity date of a credit.

**Bearer (Holder):** The last node in a credit loop is always called bearer (holder). When someone transfers a credit in a loop, that node becomes immediately an endorser.

**Endorser:** All other nodes that fall between the issuer, which is the first node in the credit loop, and the last node, and transfer the credit to the next node.

**Maturity:** The time a credit expires. It is measured as blocks in estimation. The block per day is 1440 (60 blocks an hour times 24 hours a day). Suppose a credit is for 100 days, the maturity is 1440x100, i.e. 144,000 blocks.

**Settlement:** When a credit matures, settlement is made to the holder, the last node in a loop. Settlement may be automatic or manual.

**Escrow:** Trust based parties for Protocol 2. If EscrowOn is false, then 100% collateralization is used and settlement is automatic. There is no need for escrows in protocol 1 which works as complete trustless version.

**Avalist:** Avalists support issuer or endorsers with MCL as additional colletarization and can earn with 3x staking with those support. In case of non-redemption, their funds are utilized. Avalists are available only in Protocol 2. The parameter avalcount is always zero for protocol 1.

**BlockageAmount:** This is a term for protocol 2. An issuer may be asked to put some collateralization by a holder. In that case, the issuer gets benefit of 3x staking in protocol 2.

**Dispute Expiry:** It is grace period for solving non-redemption problem in credit loops in protocol 2. An issuer may have this time as blocks when creating a credit under protocol 2 without or insufficient collateralization. Before this period expires, an escrow should do all actions according to aggrement with the issuer to solve non-redemption. Otherwise, the escrow is penalized in the system.

### Important Commands for Making Credit Loops
- ```marmararecieve```

This command is used to get a credit from an issuer or an endorser. When asking a credit from an issuer, i.e. the first node, it has a unique use. In other nodes, it is the same.

**Scenario 1:** Two nodes are making a credit loop for the first time. This credit loop may be created for a sale of a good or service in the market. In such case, the holder (the one selling the product/service) should request for a credit from the issuer (the one paying the product/service) by writing down the following command:

<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmarareceive senderpk amount currency matures '{"avalcount":"n"}'
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmarareceive senderpk amount currency matures {\"avalcount\":\"n\"}
```
</details>

>```senderpk``` is the pubkey address of the issuer (the one paying the product/service)
>
>```amount``` is the payment amount. Please note that this amount should be available in activated fund of the issuer and if not then must be activated thru ```marmaralock``` command by th issuer.
>
>```currency``` is MARMARA
>
>```matures``` is the time that respective credit expires, 60 blocks an hour times 24 hours a day making 1440 blocks per day.
>
>```'{"avalcount":"n"}'``` is the number of avalists i.e. '{"avalcount":"0"}' for protocol 1. Replace n with 0 for Protocol 1.


This marmarareceive call generates a hex code. This HEXCODE needs to be verified by the holder by executing the ```sendrawtransaction``` command:

<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL sendrawtransaction HEXCODE
```
</details>

<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL sendrawtransaction HEXCODE
```
</details>

Once this command is executed, a transaction id named ```txid``` gets generated. This ```txid``` along with the ```receiverpk``` needs to be communicated to the issuer to complete the credit loop. But, an alternative to this communication would be the use of ```marmarareceivelist``` method which could be used to see the receive requests made to the issuer himself/herself.

```marmarareceivelist``` method would be executed by the issuer by the following command:

<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmarareceivelist pubkey maxage
```
</details>

<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmarareceivelist pubkey maxage
```
</details>

> ```pubkey``` is the pubkey address of the issuer connected to the Marmara Chain and the ```maxage``` by default is 24x60
> The response of this command is a list of pair of txid's created by the respective pubkeys.

- ```marmaraissue```

This command is only used by issuer, the first node to create/issue a credit. By this, a credit is also transferred to the first holder, i.e. the second node. Many of the parameters for a credit loop is decided between the issuer and the first holder.
```marmaraissue``` method takes in the following arguments: 
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmaraissue receiverpk '{"avalcount":"n", "autosettlement":"true"|"false", "autoinsurance":"true"|"false", "disputeexpires":"offset", "EscrowOn":"true"|"false", "BlockageAmount":"amount" }' requesttxid
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmaraissue receiverpk "{\"avalcount\":\"n\", \"autosettlement\":\"true"|"false\", \"autoinsurance\":\"true"|"false\", \"disputeexpires\":\"offset\", \"EscrowOn\":\"true"|"false\", \"BlockageAmount\":\"amount\"}" requesttxid
```
</details>

>```receiverpk``` is the pubkey of the receiver which is the holder.
>
>```"avalcount":"n"``` is the number of avalists i.e. '{"avalcount":"0"}' for protocol 1. 
>
>``` "autosettlement":"true"|"false"``` AutoSettlement is true due to 100% collateralization in Protocol 1.
>
>```"autoinsurance":"true"|"false"``` Autoinsurance is true due to 100% collateralization in Protocol 1.
>
>```"disputeexpires":"offset"```  Dispute expiry is set to 0 due to 100 collateralization in Protocol 1.
>
>```"EscrowOn":"true"|"false"``` EscrowOn is set to false due to 100% collateralization in Protocol 1.
>
>```"BlockageAmount":"amount" }``` blockage amount is set to 0 due to 100 collateralization in Protocol 1.
>
>```requesttxid``` is the txid generated by the holder communicated to the issuer. 

A typical example of a ```marmaraissue``` command to complete the credit loop by the issuer is given below:
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmaraissue receiverpk '{"avalcount":"0", "autosettlement":"true", "autoinsurance":"true", "disputeexpires":"0", "EscrowOn":"false", "BlockageAmount":"0" }' requesttxid
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmaraissue receiverpk "{\"avalcount\":\"0\" \"autosettlement\":\"true\" \"autoinsurance\":\"true\" \"disputeexpires\":\"offset\" \"EscrowOn\":\"false\" \"BlockageAmount\":\"0\"}" requesttxid
```
</details>

This ```marmaraissue``` command in turn returns a hex code response, and now the issuer has to execute the sendrawtransaction method to get the transaction executed on the blockchain as follows:
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL sendrawtransaction HEXCODE
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL sendrawtransaction HEXCODE
```
</details>

This creates a credit loop between the issuer and the holder. The credits locked in a loop can be circulated to buy things during shopping. The issuer and the holder get 3 times of chances of staking on the MCL funds until the maturity of the credit loop.
- ```marmaracreditloop```

To display the credit loop between the issuer and the holder, the following ```marmaracreditloop``` command may be executed:
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmaracreditloop txid
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmaracreditloop txid
```
</details>

> ```txid``` is the baton transfer id of the Marmara Credit Loop.

- ```marmaratransfer```

**Scenario 2:** The holder from the previous scenario wishes to utilize the coins locked in loop by buying goods/services on the same credit loop created earlier. For such case, when the holder transfers a credit in a loop, that node immediately becomes an endorser. And in such way, the last node in a credit loop is always called the bearer (holder). In other words, all endorsers are previously holders.
One should bear in mind that endorsers lose the 3x staking power when a credit is transferred to a new holder.

For this purpose, the new holder makes a ```marmarareceive``` request to the endorser to get the credit for selling the goods/services by the following command:
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmarareceive senderpk batontxid '{"avalcount":"n"}'
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmarareceive senderpk batontxid {\"avalcount\":\"n\"}
```
</details>

>```senderpk``` is the pubkey address of the endorser (the one buying the product/service)
>```batontxid``` is the baton transaction id of the previously created credit loop
>```{"avalcount":"n"}``` is the number of avalists i.e. {"avalcount":"0"} for protocol 1.

This marmarareceive call generates a hex code. This HEXCODE needs to be verified by the new holder by executing the ```sendrawtransaction``` command:
<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL sendrawtransaction HEXCODE
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL sendrawtransaction HEXCODE
```
</details>

Once this command is executed, a transaction id named ```txid``` gets generated. This ```txid``` along with the ```receiverpk``` needs to be communicated to the endorser to complete the credit loop. But, an alternative to this communication would be the use of ```marmarareceivelist``` method which could be used by the endorser to see the receive requests made to himself/herself.
Then, the endorser executes the following ```marmaratransfer```command to get the credits transferred to the new holder:

<details>
    <summary> Linux: </summary>
    
```
./komodo-cli -ac_name=MCL marmaratransfer receiverpk '{"avalcount":"n"}' requesttxid
```
</details>
<details>
    <summary> Windows: </summary>
    
```
komodo-cli.exe -ac_name=MCL marmaratransfer receiverpk {\"avalcount\":\"n\"} requesttxid
```
</details>

>```receiverpk``` is the pubkey of the receiver which is the new holder.
>
>```"avalcount":"n"``` is the number of avalists i.e. '{"avalcount":"0"}' for protocol 1 (n=0). 
>
>```requesttxid``` is the txid generated by the new holder communicated to the endorser. 

Then the endorser executes the ```sendrawtransaction``` command with the hex code resulting from ```marmaratransfer```command.

>Please Note that ```sendrawtransaction``` command is used after ```marmarareceive```, ```marmaraissue```, ```marmaratransfer``` and ```marmaralock``` to make the results of commands to be executed on blockchain. The usage is presented throughout the scenario 1 and 2.

**In this way, the credit loops can circulate up to 1000th node _within_ the maturity time to buy goods/services.**

References
---
For more detailed information on how Marmara Credit Loops work, kindly refer to detailed article [here](https://medium.com/@drcetiner/how-marmara-credit-loops-mcl-work-31d1896190a5).
