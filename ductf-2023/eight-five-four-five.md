---
description: 'Author: Karjun (g00bert)'
---

# Eight Five Four Five

Medium article link: [https://g00bert.medium.com/eight-five-four-five-1562d95684ee](https://g00bert.medium.com/eight-five-four-five-1562d95684ee)

Challenge Description: Warming up, let’s get you setup and make sure you can connect to the blockchain infra ok :). Your challenge is to ensure the isSolved() function returns true!

Challenge Author: Blue Alder

Category: Blockchain (Beginner)

This is how I solved this challenge as someone with barely any knowledge in blockchain.

## Basic Recon <a href="#daec" id="daec"></a>

After launching an instance for the challenge, we are greeted by:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*ey-qDO0mDejTT8Um5_pFwA.png" alt="" height="352" width="700"><figcaption></figcaption></figure>

It appears to be a smart contract we need to interact with along with some details we might need to use to interact with the smart contract.

According to the challenge description, our goal is to get the isSolved() function to return true.

To achieve that goal, lets look through the code.

Based on the code with get a few ideas:

1. The contract is constructed with some\_string (possibly unknown)
2. solve\_the\_challenge\_function takes an input and compares it against the an initialized string, if its the same the overwrites you\_solved\_it to become true.

So we probably mainly need to invoke the solve\_the\_challenge function and input the string contained inside the use\_this variable.

## Interacting with the smart contract <a href="#144b" id="144b"></a>

Now that we sort of know what to do, lets actually interact with the contract. The problem is I know very little about this.

Lets consult our good friend ChatGPT.

So i prompted ChatGPT with:

> Given this code, how do i interact with it so i can make is\_solved return true using the web3 package in python: // SPDX-License-Identifier: MIT pragma solidity ^0.8.19;
>
> contract EightFiveFourFive { string private use\_this; bool public you\_solved\_it = false;
>
> constructor(string memory some\_string) { use\_this = some\_string; }
>
> function readTheStringHere() external view returns (string memory) { return use\_this; }
>
> function solve\_the\_challenge(string memory answer) external { you\_solved\_it = keccak256(bytes(answer)) == keccak256(bytes(use\_this)); }
>
> function isSolved() external view returns (bool) { return you\_solved\_it; }
>
> }
>
> Here is some provided information to interact with the contract: EightFiveFourFive Goal: have the isSolved() function return true
>
> Status: DEPLOYED Player Balance: 1.0 ETH
>
> Player Wallet Address: 0x246C07C750Dc39d327b1681f400ab5FeC6C949eC
>
> Private Key: 0xbeaf93a56da83fd5bf1c8adac28346ff80e2a461e2c64a7f76d0884e3966c544
>
> Contract Address: 0xf22cB0Ca047e88AC996c17683Cee290518093574
>
> Block Time: 0
>
> RPC URL: https://blockchain-eightfivefourfive-1647b28dc556ab41-eth.2023.ductf.dev:8545
>
> Chain ID: 31337

I pretty much dumped all the information provided to me and asked ChatGPT to do it.

\*Also note I just fed my Private Key directly to ChatGPT which is generally a bad idea, but since its a throw away one that is only used for this challenge, it did not matter too much.

So initially, it provided me with the following code:

Initially it gave me this code

```
from web3 import Web3
```

```
rpc_url = "<https://blockchain-eightfivefourfive-1647b28dc556ab41-eth.2023.ductf.dev:8545>"
web3 = Web3(Web3.HTTPProvider(rpc_url))
contract_address = "0xf22cB0Ca047e88AC996c17683Cee290518093574"
contract_abi = [
    {
        "inputs": [
            {
                "internalType": "string",
                "name": "some_string",
                "type": "string"
            }
        ],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "inputs": [],
        "name": "isSolved",
        "outputs": [
            {
                "internalType": "bool",
                "name": "",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "readTheStringHere",
        "outputs": [
            {
                "internalType": "string",
                "name": "",
                "type": "string"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    },
    {
        "inputs": [
            {
                "internalType": "string",
                "name": "answer",
                "type": "string"
            }
        ],
        "name": "solve_the_challenge",
        "outputs": [],
        "stateMutability": "external",
        "type": "function"
    },
    {
        "stateMutability": "payable",
        "type": "fallback"
    }
]contract = web3.eth.contract(address=contract_address, abi=contract_abi)
is_solved = contract.functions.isSolved().call()
print("isSolved() =", is_solved)
private_key = "0xbeaf93a56da83fd5bf1c8adac28346ff80e2a461e2c64a7f76d0884e3966c544"
account = web3.eth.account.from_key(private_key)
web3.eth.default_account = account.address
answer = "your answer here"
transaction = contract.functions.solve_the_challenge(answer).build_transaction({
    'from': account.address,
    'nonce': web3.eth.get_transaction_count(account.address),
    'gas': 100000,
    'gasPrice': web3.eth.gas_price
})
signed_txn = web3.eth.account.sign_transaction(transaction, private_key=private_key)
transaction_hash = web3.eth.send_raw_transaction(signed_txn.rawTransaction)
transaction_receipt = web3.eth.wait_for_transaction_receipt(transaction_hash)print("Transaction hash:", transaction_hash.hex())
is_solved = contract.functions.isSolved().call()
print("isSolved() =", is_solved)
```

When i tried running it, this happened:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*bcvonSPX1Um_TKg8KknXuA.png" alt="" height="127" width="700"><figcaption></figcaption></figure>

It seems that we are indeed interacting with the smart contract, but isSolved is not returning True.

So we can just add the lines:

```
string = contract.functions.readTheStringHere().call()
print(string)
```

This allows us to print the string asreadTheStringHere isalready defined in the ABI ChatGPT graciously provided earlier.

So after printing the string to be passed into solve\_the\_challenge function and modifying the answer variable of the script:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*C4rQr02tHHCTl8QKHmqeCw.png" alt="" height="140" width="700"><figcaption></figcaption></figure>

We manage to get isSolved to return True.

After that we just return to the challenge instance and hit get the flag.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*-RclN98FZrdm036FoqTXZg.png" alt="" height="388" width="700"><figcaption></figcaption></figure>

And there we have it!
