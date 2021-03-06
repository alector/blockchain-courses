# **Introduction blockchain Ethereum**

## **DLT: Distributed ledger technology AKA Blockchain**

Blockchain technologies solved 2 problems:

- immutability: data can't be altered
- distributed process: no more a single point of authority or a single point of failure (`SPOF`), each nodes work together based on a consensus mechanism preventing malicious actors.

### **Immutability**

In a blockchain all transactions are stored in a block.
Each block includes the `cryptographic hash` of the prior block in the chain, linking the two blocks, and also a `cryptographic hash` of all the transactions stored in the block.  
The `cryptographic hash`, is a `hash` generated by a `cryptographic hash function`.  
The `cryptographic hash function` is a one way function (non invertible function) that maps the content of a block to a number.  
These 2 numbers, the `cryptographic hash` of the previous block and the `cryptographic hash` of all the confirmed transactions in the block, are stored in the `block header` of the new incoming block.  
![blockchain](../res/blockchain.jpeg)
Bellow how this kind of process can be implemented with code:
![code structure of blockchain](../res/codeview.jpg)
Bellow a basic overview of a block header:  
![block header basic overview](../res/blockheader.png)

This process confirms the **integrity** of the block, all the way back to the original `genesis block`.

If a malicious actor want to alter a block, for example by changing a transaction amount, he will alter the `hash` of all the transactions within the block. So he will have to mine again this block. The process will alter the whole block `hash`, and all the following blocks will have a different `hash` stored in their header for the previous block. So the malicious actor will have to mine also all the following blocks based on the `hash` of the malicious crafted block.  
The other protection which prevent these kind of modifications is a consensus mechanism.
Attackers would need to control 51% of the network to reverse transactions that have already taken place in a blockchain.
This is known as the **51% Attack**
Needless to say that this is actually impossible, attackers would need to much computational power, and even if they succeed it would invalidate trust on this blockchain and users will just leave.

### **Centralized vs Decentralized vs Distributed**

![Centralized vs Decentralized vs Distributed](../res/CDD.png)

In a Distributed network there is not a single (or even multi) point of authority.  
The objective is to avoid a single point of failure: `SPOF`
So consensus mechanisms are used to make sure that each nodes in a distributed network work together, and never in a malicious way.  
In the case of a blockchain network, consensus mechanisms are used to control how a blockchain transaction can be validated, written in a block and executed.

### **Consensus mechanisms**

#### **Byzantine generals problem**

https://fr.wikipedia.org/wiki/Probl%C3%A8me_des_g%C3%A9n%C3%A9raux_byzantins

#### **Proof of work: PoW**

In french: **preuve de travail**.  
This is the original consensus algorithm.  
With `PoW`, miners compete against each other to complete transactions on the network and add new blocks to the blockchain.  
Miners have to solve a difficult puzzle using their computers processing power.  
The mathematical puzzle to solve consists in finding a hash number for the current block header by manipulating the `nonce` field of this block.  
The hash number to find has to be less than a `target` hash. The `target` hash is defined by a `difficulty`.  
![mining process](../res/bitcoin_block_hashing.jpg)

The first miner to solve the puzzle is given a reward for their work.  
A proof of work is a piece of data which is difficult (costly, time-consuming) to produce but easy for others to verify and which satisfies certain requirements.

#### **Proof of stake: PoS**

In french: **preuve d'enjeu** ou **preuve de participation**.  
Proof of stake (`PoS`) is a type of consensus algorithm by which a cryptocurrency blockchain network aims to achieve distributed consensus.  
In PoS-based blockchains the creator of the next block is chosen via various combinations of random selection and wealth or age (i.e., the stake).
In the case of the Ethereum blockchain, there will be a minimum threshold of 32 ETH required to participate in staking, and validators will need to be running a validator node. This doesn’t need to be specialist machinery and could be done on a consumer-grade computer or laptop. However, validators will be expected to be online consistently or face minor penalties.  
In the case of blockchain operating a `PoS` consensus, miners/validators/stakers can mine or validate block transactions based on the amount of cryptocurrencies they holds.  
In order to add a malicious block, an attacker would have to own 51% of all the cryptocurrency on the network.

Ethereum has historically operated a proof-of-work consensus. However, one reason for moving to proof-of-stake is that it’s generally considered to be far more energy-efficient than proof-of-work.

### **Mining**

The Miner nodes create blocks in the chain.  
A block is a data structure that contains a set of transactions. When creating a block, the miner will select some transactions from its pool of pending transactions (transactions waiting to be included in the chain) and start mining the block.  
The important thing to know is that mining is an expensive process. Therefore, if miners didn’t get anything in return for mining, no one would do it.
In Ethereum, when a miner mines a new block, it receives the fees from all transactions included in this block and a block reward (actually 2 ETH). Therefore, the higher the gasPrice in the transactions, the higher the fees that the miner receives will be.

### **Fees/Gas**

The fees on Bitcoin blockchain, or the `Gas` system on Ethereum blockchain, are a protection system and reward for nodes processing the transaction.
Computing costs money:

- hosting a service
- storing data
- processing information

Each transaction that modifies a state in a blockchain, like sending cryptocurrencies, deploying a smart contract or changing a value in a smart contract, will cost the sender some fees.

Another aspect of charging the user for their actions in the network is to prevent abuses. If you are paying for every operation you execute, you’ll do your best to implement your code in the most efficient way. The fee cost also prevents bad actors from flooding the system with useless operations (unless they are willing to spend a lot of money to execute useless code).

### **asymmetric cryptography**

The core of asymmetric cryptography is the usage of public and private key pairs.
A private key is a random number. The associated public key is a number generated by a one way algorithm based on the private key.
This algorithm is an elliptic curve digital signature algorithm (`ecdsa`).
The elliptic curve used by Bitcoin, Ethereum, and many other cryptocurrencies is called secp256k1. The equation for the secp256k1 curve is `y² = x³+7`. This curve looks like:

![ecdsa](../res/ecdsa.gif)

A private key is a big number, preferably, randomly generated.
The private key has to be kept secret.  
A public key is a big number obtained by an `ecdsa` on the private key.
The public key can be shared with anyone without compromising security.  
An address is obtained from a public key with a hash function.  
A transaction contains the message of the transaction, and a signature of this message generated with the private key of the sender.
Anyone can verify the generated signature to:

- recover the public key and address of the signer
- verify the integrity of the message, that it is the same message that was signed by the signer.

With the signature and the hash of the original data we can perform an `elliptic curve signature recovery` and get the public key and then the address. If the address recovered is identical to the address of the sender, then the private key holder of the public key pair did indeed sign the message.

## **Ethereum**

### **Ethereum vs Bitcoin**

Bitcoin white paper: https://bitcoin.org/bitcoin.pdf  
Ethereum yellow paper: https://ethereum.github.io/yellowpaper/paper.pdf

Technically Ethereum and Bitcoin follow the same scheme in their implementation, but there are 3 main differences between these 2 blockchains:

- Ethereum has a virtual machine which can execute instructions and store data.
- Ethereum offers the possibility of deploying and using `smart contracts`, an enhanced version of the Bitcoin `Script`. The instructions of `smart contracts` are executed in the `EVM` and data are stored in the `EVM`.
- Ethereum introduced `Gas`, an enhanced system of miners fees on Bitcoin.

### **EVM**

The Ethereum Virtual Machine (`EVM`) is a powerful, sandboxed virtual stack embedded within each full Ethereum node, responsible for executing contract bytecode. Contracts are typically written in higher level languages, like Solidity, then compiled to `EVM` bytecode.  
This means that the machine code is completely isolated from the network, filesystem or any processes of the host computer. Every node in the Ethereum network runs an `EVM` instance which allows them to agree on executing the same instructions. The `EVM` is Turing complete, which refers to a system capable of performing any logical step of a computational function.
For every instruction implemented on the `EVM`, a system that keeps track of execution cost, assigns to the instruction an associated cost in Gas units. When a user wants to initiate an execution, they reserve some Ether, which they are willing to pay for this gas cost.  
List of the EVM opcodes: https://ethervm.io/

### **Smart contracts**

A smart contract is a self-executing contract with the terms of the agreement between buyer and seller being directly written into lines of code.
Smart contracts permit trusted transactions and agreements to be carried out among disparate, anonymous parties without the need for a central authority, legal system, or external enforcement mechanism.
A smart contract is a set of functions and data stored in the EVM.  
Smart contract can execute instructions, but are limited by a small list of available opcodes and also by the `Gas limit`, the maximum amount of `Gas` a user is willing to pay.

### **Gas**

Only read-only transactions are free. Else the sender has to pay for the amount of `Gas` needed when he sends a transaction to the Ethereum blockchain.  
There are 3 main use cases of `Gas`:

- reward for the miner who mined the block. He will earn all the `Gas` cost spent for all the transactions per block mined.
- avoid `DDoS` attacks. As Transactions need an amount `Gas`, a `DDoS` attack need a huge amount of cryptocurrencies for paying this `Gas` cost.
- protection of the user. The `gasLimit` is used to protect the user from wasting his Ether because of a bug in a smart contract or estimation errors.

The well known analogy to understand `Gas` is car and fuel.  
If you own a car, and you need to drive it from point A to point B, you need an amount of fuel. In the same way, if you have some operations that you want to execute in the Ethereum EVM, you need `Gas`. With your car, the further you drive, the more fuel you need. In Ethereum, the more you compute, the more `Gas` you need.
The amount of `Gas` needed is specified in the Appendix G of the Yellow Paper.

The `gasPrice` is the value that the transaction sender is willing to pay per `Gas` unit.
Following the car/fuel analogy, if your car has a 50 liter tank, how much do you pay to completely fill the tank? The answer depends on the price per liter in the pump.  
It is the same with Ethereum and `Gas`, if you have a transaction that needs 10 gas to execute, the price you pay to execute that transaction depends on the price per unit of gas.

The `gasLimit` is the maximum `Gas` that the transaction sender is willing to spend executing that transaction. Sometimes when executing a transaction, you might not know exactly how much it is going to cost. Imagine a scenario where you have a smart contract with a bug, an infinity loop. Without a gasLimit, it would be possible to consume the whole balance of the sender account. The `gasLimit` is a safety mechanism to prevent someone from using all their Ether due to a bug or an estimation error.

So when a user sends a transaction he will pay a first amount of:  
**Intial cost = `gasPrice` \* `gasLimit`**  
If the intrinsic cost is higher than the balance of the sender account, the transaction is considered invalid. After the transaction has been processed, any unused gas is refunded to the sender account. So a user will pay when the transaction has ben processed:  
**Real cost = `gasPrice` \* `gasUsed`**  
However, if your transaction runs out of gas during execution, there is no refund. That is why usually the transaction sender sets the gasLimit to higher than the estimated amount of gas.

If the Ethereum network is not congested, costs and `gasPrice` are cheap. This is why they are expressed in a smaller denomination than Ether:

| Unit                | Wei Value | Wei                       |
| ------------------- | --------- | ------------------------- |
| wei                 | 1 wei     | 1                         |
| Kwei (babbage)      | 1e3 wei   | 1,000                     |
| Mwei (lovelace)     | 1e6 wei   | 1,000,000                 |
| Gwei (shannon)      | 1e9 wei   | 1,000,000,000             |
| microether (szabo)  | 1e12 wei  | 1,000,000,000,000         |
| milliether (finney) | 1e15 wei  | 1,000,000,000,000,000     |
| ether               | 1e18 wei  | 1,000,000,000,000,000,000 |

<br />
A miner will always prioritize transactions with higher Gas cost.  
People sending transactions specify a gas price, and miners decide which transactions to mine into a block. The two meet somewhere in the middle on a price.

When sending a transaction, it can be hard to know what is the minimum gasPrice at that moment. There are some tools that scan the network and the average gasPrice used in recent transactions to help with choosing a fair gasPrice that is likely to be accepted by miners:  
ETH Gas station: https://ethgasstation.info/  
Etherscan Gas tracker: https://etherscan.io/gastracker  
Browser addons: https://addons.mozilla.org/en-US/firefox/addon/ethereum-gas-price-extension/

## **MetaMask**

A crypto wallet & gateway to blockchain apps

### **Installation**

Download from MetaMask browser extension from: https://metamask.io/

### **Configuration**

Setup a password and keep your seed phrase in a safe place.

### **Networks**

5 known networks:

- Mainnet => The real Ethereum network, no joke here!!!
- Ropsten
- Kovan
- Rinkeby
- Goerli

We can also configure personal networks, it will be useful when we will run our local `ganache` node.

Networks comparison: https://ethereum.stackexchange.com/questions/27048/comparison-of-the-different-testnets

### **Faucets**

All test networks provide faucets platform for getting testnet Ethers.

- Ropsten: https://faucet.ropsten.be/
- Kovan: https://faucet.kovan.network/
- Rinkeby: https://faucet.rinkeby.io/
- Goerli: https://goerli-faucet.slock.it/

### **web3**

While using MetaMask as a browser extension, the `web3` environment is injected into the browser. This way the browser can perform blockchain operation on Dapp through the MetaMask extension.  
MetaMask injects a global API into websites visited by its users at `window.ethereum`. This API allows websites to request users' Ethereum accounts, read data from blockchains the user is connected to, and suggest that the user sign messages and transactions. The presence of the provider object indicates an Ethereum user. MetaMask dev recommend using **detect-provider** package to detect our provider, on any platform or browser.
**detect-provider**: https://www.npmjs.com/package/@metamask/detect-provider
You can check in your browser console that `window.ethereum` and `window.web3` objects exist.  
Soon the `window.web3` will be removed following the MetaMask api documentation:
https://docs.metamask.io/guide/ethereum-provider.html#window-web3-removal  
So `window.ethereum` is the API to use for interacting with a browser.

## **Transaction**

### **First transaction with MetaMask**:

1. Create a second account in your MetaMask wallet.
2. Copy the Ethereum address of this new account in your clipboard.
3. Select an account that has testnet Ethers.
4. Click `Send`.
5. Copy the address of your second account in `Add Recipient`.
6. Select an amount to send, for example 1 `ETH`.
7. Click next and confirm. And wait a confirmation from MetaMask confirming your transaction.

### **Analysis of a transaction**:

In MetaMask, under `Activity` we can find an history of sent, received and rejected transactions. But there is not so much information we can read from there.
For getting a deeper overview of our transaction we can go on **Etherscan**.  
From MetaMask there is a link for viewing a specific transaction on Etherscan directly.
Etherscan website: https://etherscan.io/
Etherscan is an Ethereum explorer for getting informations on blocks, transactions, addresses and even smart contracts code.
You have to browse the Etherscan version of the network you want to get information from.

## **Remix**

Access to remix: https://remix.ethereum.org  
Remix is a web IDE (like CodePen for Javascript).
On remix we can write, deploy and depoy a contract. We can also interact with a deployed contract.  
Remix is pretty good for learning `Solidity` and test quickly small smart contracts, but it is not an option for serious project.

_remix live coding..._

## **Ganache**

Ganache is a local development Ethereum node, for fast deployment and testing.

### **Switch back to node 12**

ganache-cli doesn't work on node 14 yet. So we need to switch back to node 12.

check your node version with:

```zsh
% node --version
v12.19.0
```

If the output doesn't show you `v12.19.0` please follow the instructions below.

#### **Linux and WSL**:

On Linux and WSL just use `nvm` to install and switch to node `12.19.0 LTS`.

#### **Macos**:

Check available node versions:

```zsh
% brew search node
==> Formulae
libbitcoin-node         node ✔                  node-sass               node@12                 nodebrew                nodenv
llnode                  node-build              node@10                 node_exporter           nodeenv
```

Unlink current version:

```zsh
% brew unlink node
```

Install node@12:

```zsh
% brew install node@12
```

Link it:

```zsh
% brew link node@12
```

Check the last message output from the previous command, i had to add the of node in my _~/.zshrc_

```
If you need to have this software first in your PATH instead consider running:
  echo 'export PATH="/usr/local/opt/node@12/bin:$PATH"' >> ~/.zshrc
```

```zsh
% echo 'export PATH="/usr/local/opt/node@12/bin:$PATH"' >> ~/.zshrc
% source ~/.zshrc
```

Check your current version:

```zsh
% node --version
v12.19.0
```

### **Install ganache**

Install with `yarn`:

```zsh
% yarn global add ganache-cli
```

To have access to Yarn’s executables globally, you will need to set up the PATH environment variable in your terminal. To do this, add the line below into your profile, in my case my profile is in `~/.zprofile`:

````zsh
export PATH="\$PATH:`yarn global bin`"
```

and then load your profile, in my case `~/.zprofile`:
```zsh
% source ~/.zprofile
````

### **Execution**

Simply run the `ganache-cli` from the command line.

```
% ganache-cli
```

## **Deploy a Smart contract from Remix**

A simple hello world smart contract:

_Hello.sol_:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.7.3;                      // version du compilateur
contract HelloWorld {                        // Definition de notre contrat
     string public hello = "Hello world!";  // unne variable public de type string
}
```

### **First smart contract on browser**

### **First smart contract on ganache**

### **First smart contract on a testnet**

### **SimpleStorage.sol**

Deploy and use this smart contract:
_SimpleStorage.sol_:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.8.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

## Glossary:

`Block header`: A data structure containing all information of a block. It is used as an identifier of a block  
`Block hash`: A Block Hash is a reference number for a block in the blockchain. You get a Block Hash by hashing the block header  
`Block reward`: The reward a miner get if he successfully mines a block. Actually 2 ETH on Ethereum  
`BTC`: Bitcoin cryptocurrency  
`DAO`: Decentralized Autonomous Organization  
`cryptographic hash`: see `hash`  
`cryptographic hash function`: see `hasing function`  
`Difficulty`: This describes how difficult, in relation to the genesis block, the target will be to reach.  
`DDoS`: Distributed Denial of Service  
`ECDSA`: Elliptic Curve Digital Signature Algorithm  
`EIP`: Ethereum Improvement Proposals
`ETH`: Ethereum/Ether cryptocurrency  
`EVM`: Ethereum Virtual Machine  
`Gas`:  
`gasLimit`:  
`gasPrice`:  
`Genesis block`: The first block of a blockchain `hash`: A unique identifier of a data. `hashing function`: take an input, file or data, and generate a `hash`of this input `PoW`: Proof of Work `PoS`: Proof of Stake `SPOF`: Single Point Of Failure `Target`: The number that the block hash must be less than in order to be valid
