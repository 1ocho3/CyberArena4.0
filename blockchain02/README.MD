# CYBER ARENA 4.0 CHALLENGE
## BLOCKCHAIN - VULNERABLE CONTRACTS
### nocnoc37
###### solidity - abi - ethereum - blockchain - remix - metamask
---
### CONTENT
---  
THE CHALLENGE BEGS THE FOLLOWING QUESTION:  

"***WILL YOU BE "SMART" ENOUGH TO INTERACT WITH THIS "CONTRACT"?***  
***THERE ARE 300 DIFERENT ACCOUNTS TO AVOID USER OVERLAPING. EACH ACCOUNT HAS ITS PRIVATE KEY TO SIGN TRANSACTIONS, AND EACH ACCOUNT HAS 100 ETH WITHIN THE LOCAL BLOCKCHAIN.***  
***ctf.cyb3r4rena.com:8545***" 

---  

We are given the following files:  

-The contract deployed on the local blockchain, `contract.sol`, written in Solidity language.  
-What appears to be two text files `ABI_and_contract.txt` and `accounts.txt`.  
    -`ABI_and_contract.txt` holds the address to the contract.sol deployed on `ctf.cyb3r4rena.com:8545` and the ABI content necessary to interact with the  contract. I will get into how smart contracts work later on.  
    -`accounts.txt` holds 299 diferent accounts and private signatures, to try to prevent participants from using the same account at the same time.  

![content](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/content.png?raw=true)    

---
HOW TO INTERACT WITH A SMART CONTRACT?  

First we need to understand how they are deployed. The blockchain has its users, and they can deploy their own contracts (programs written on blockchain specific language). 

Deployed contracts have an address to refer to them when you want to connect. Normally in adittion to the address, to interact with some contracts you might need a configuration file, in this case, the ABI content on the `ABI_and_contract.txt` file.
Let's open `ABI_and_contract.txt`:  

![abi-and-contract](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/abi-and-contract-content.png?raw=true)

We can use an online tool to convert the ABI to .sol for easier understanding. For now, it's only necessary to know that the contract address and the ABI file, are the two keys that bring access to the contract.  
[ABI to SOL online tool](https://gnidan.github.io/abi-to-sol/)  

```s
pragma solidity >=0.7.0 <0.9.0;

interface MyInterface {
    function getFlag() external view returns (string memory);

    function getIdGodRole(address user) external view returns (uint256);

    function getRole(address user) external view returns (uint256);

    function roles(address) external view returns (uint256);

    function setRole(address user, uint256 role) external;

    function setStringResponseAddress(address stringResponseAddress_) external;
}
```

With this we know:  
-There is a function called `getFlag` which returns a string.  
-There is a function called `getRole` which seems to return the role of a given address.  
-There is a function called `setRole` which seems to set a given role to a given address  
-There is a function called `getIdGodRole` which we can infer returns the ID of the godRole if you give it an address.  

---
Now let's open `contract.sol`, and read it.
After a thorough analysis, we can see that the vulnerability consist on that the deployer wrote an "OR" `||` logical operator instead of an "AND" `&&`.  

This address in the `setStringResponseAddress()` is the deployer of the contract. This means that this address may be one which has high privileges and roles. (Bear that in mind)  
```s
function setStringResponseAddress(address stringResponseAddress_) public {
        require(msg.sender == 0x7955923248F7EFB8F7BFfF8D8b25797a6493653d;
        stringResponseAddress = stringResponseAddress_;
    }
```

Therfore anyone can use the `setRole()` function as long as they give the same address as the one they are using.  

```s
function setRole(address user, uint role) public {
        require(msg.sender == user || roles[msg.sender] > 0, "Access denied.");
        roles[user] = role;
    }
```  

The `getIdGodRole()` function, requires us to have the role "1" set up in our address.
```s
function getIdGodRole(address user) public view returns (uint) {
        require(roles[user] > 2, "Access denied."); // only accessible by role 1
        return godRole;
    }
```

To conclude our analysis, the `getFlag()` function requires us to have "godRole".  
```s
function getFlag() public view returns (string memory) {
        require(roles[msg.sender] == godRole, "Access denied."); // only accessible by GodRole
        flag = REDACTED; // get flag
        return flag; // return flag
    }
```
###### NOTE THAT SENSIBLE INFORMATION WAS REDACTED ON PURPOSE BY THE CTF TEAM, TO ALLOW US TO UNDERSTAND HOW THE CONTRACT WORKS, KNOWING THE LEAST POSSIBLE INFORMATION.

---
After the analysis, the path to get the flag gets clear.  
-First we would need to set our own role as 1  
-Then we should use `getIdGodRole()` from an address that might have godRole. (The deployer account may have godRole)
-After getting the ID and making our address have godRole, we just need to call `getFlag()` to get our flag.

---
For this, we need a crypto-wallet. I recommend you install [METAMASK](https://metamask.io/download/), and an ethereum IDE like [REMIX](https://remix.ethereum.org).  

Create a new wallet in metamask, and import one of the 299 accounts, by entering it's private key:  

![metamask](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/metamask-import-account.png?raw=true)  

---
Now switch to the CTF local blockchain `ctf.cyb3r4rena.com:8545`  
Be sure to put the adecuate chainID, Metamask gives it to you after introducing anything that doesn't correspond with it.  
ChainID = 1337  
Currency = ETH  

![metamask-switching](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/metamask-setting-network.png?raw=true)  

---
100 ETH should appear in your account right after succesfully login into the network.  

![100ETH](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/100ETH.png?raw=true)  

---
Hop on to Remix, create a new file called `abi.abi`. Inside of it, copy the ABI content:  

![abi.abi](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/remix-create-new-file.png?raw=true)  

Head to "Deploy and Run" and set the Enviroment to "Injected Provider - Metamask".  
It will ask you to connect with your metamask accounts, we need to check the one imported from the CTF and accept the connection:

![metamask-injected provider](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/metamask-injected-provider.png?raw=true)  

---
Important! With the `abi.abi` file **selected** go to "At Address" and introduce the contract address and confirm. After this you'll we offered to interact with the contract:  

![address-contract](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/contract-address.png?raw=true)  

---  
Now set your role to 1, get the godRole ID, set it to your account and get the flag:  

![roleto1](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/contract.png?raw=true)  
![get the flag](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/flag%20encrypted.png?raw=true)

---
Turns out the flag was base64 encoded. Cyberchef returns us the plain text format:  

![flagdecrypted](https://github.com/1ocho3/CyberArena4.0/blob/main/blockchain02/readme-required/flag%20decoded.png?raw=true)
