# Ethernaut walkthrough
![Ethernaut](https://github.com/0xVinum/Ethernaut-walkthrough/assets/138346100/27dd7a7c-76a5-4052-b670-370035445a59)
### Introduction
_"Ethernaut is a Web3/Solidity based wargame inspired by overthewire, to be played in the Ethereum Virtual Machine. Each level is a smart contract that needs to be 'hacked'._

_The game acts both as a tool for those interested in learning ethereum, and as a way to catalogue historical hacks as levels. There can be an infinite number of levels and the game does not require to be played in any particular order."_
## About
- [Introduction](#introduction)
- [About](#about)
- [Challenge](#challenge)
- [Understanding the contract](#understanding-the-contract)
  - [Code](#code)
  - [Analysis](#analysis)
  - [Exploit](#exploit)
- [Solution](#solution)

## Challenge
Look carefully at the contract's code below.

You will beat this level if

1. you claim ownership of the contract
2. you reduce its balance to 0

Things that might help

- How to send ether when interacting with an ABI
- How to send ether outside of the ABI
- Converting to and from wei/ether units
- Fallback methods
## Understanding the contract
### Code
```// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {

  mapping(address => uint) public contributions;
  address public owner;

  constructor() {
    owner = msg.sender;
    contributions[msg.sender] = 1000 * (1 ether);
  }

  modifier onlyOwner {
        require(
            msg.sender == owner,
            "caller is not the owner"
        );
        _;
    }

  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }

  function getContribution() public view returns (uint) {
    return contributions[msg.sender];
  }

  function withdraw() public onlyOwner {
    payable(owner).transfer(address(this).balance);
  }

  receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
}
```
### Analysis
1. **Pragma Statement:**
The pragma statement specifies the version of the Solidity compiler that the contract should be compiled with. In this case, the contract is intended to be compiled using Solidity version 0.8.0.

```
pragma solidity ^0.8.0;
```


3. **Contract Definition:**
The contract is named **Fallback**. It contains functions and state variables that define its behavior.

```
contract Fallback {
```

4. **State Variables:**
   - **contributions:** This is a state variable that maps addresses to their contributed amounts (balances) in Ether.
   - **owner**: This is a public state variable of type **address** that represents the owner of the contract.

```  
    mapping(address => uint) public contributions;
    address public owner;
```
5. **Constructor:**
The constructor is executed when the contract is deployed. It sets the initial **owner** to the address that deployed the contract and assigns an initial contribution of 1000 Ether to the owner.
```
  constructor() {
    owner = msg.sender;
    contributions[msg.sender] = 1000 * (1 ether);
  }
```
7. **Modifier (onlyOwner)**:
This modifier restricts certain functions to be callable only by the contract's owner. If the condition in the **'require'** statement is not met, the function execution is halted.
```
  modifier onlyOwner {
    require(
      msg.sender == owner,
      "caller is not the owner"
    );
    _;
  }
```

9. **'contribute' Function:**
This function allows users to contribute Ether to the contract. It imposes a limit of 0.001 Ether per contribution. The function updates the contributions of the sender and may update the **'owner'** if their contribution surpasses the current owner's contribution.
```
  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }
```

11. **'getContribution' Function**:
This function returns the contribution balance of the caller.
```
  function getContribution() public view returns (uint) {
    return contributions[msg.sender];
  }
```

13. **'withdraw' Function:**
This function allows the contract's owner to withdraw the contract's balance. Only the owner can call this function due to the **'onlyOwner'** modifier.
```
  function withdraw() public onlyOwner {
    payable(owner).transfer(address(this).balance);
  }
```

15. **'receive' Function:**
This function is a fallback function that is automatically called when Ether is sent to the contract without specifying a function to call. It requires a positive value of Ether and that the sender has made contributions. It updates the **'owner'** to the sender's address.
```
  receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
```
### Exploit
## Solution

