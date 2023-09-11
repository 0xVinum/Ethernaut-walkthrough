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
Claim ownership of the contract below to complete this level.
## Understanding the contract
### Code
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import 'openzeppelin-contracts-06/math/SafeMath.sol';

contract Fallout {
  
  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;


  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }

  modifier onlyOwner {
	        require(
	            msg.sender == owner,
	            "caller is not the owner"
	        );
	        _;
	    }

  function allocate() public payable {
    allocations[msg.sender] = allocations[msg.sender].add(msg.value);
  }

  function sendAllocation(address payable allocator) public {
    require(allocations[allocator] > 0);
    allocator.transfer(allocations[allocator]);
  }

  function collectAllocations() public onlyOwner {
    msg.sender.transfer(address(this).balance);
  }

  function allocatorBalance(address allocator) public view returns (uint) {
    return allocations[allocator];
  }
}
```
### Analysis

### Exploit


## Solution
