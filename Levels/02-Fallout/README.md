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
    - [Overview](#overview)
    - [Functions](#functions)
    - [Summary](#summary)
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
The "Fallout" smart contract is designed to manage the allocation and distribution of Ether on the Ethereum blockchain. It provides a mechanism for an owner to allocate funds to various addresses and allows those addresses to withdraw their allocated Ether. Additionally, the contract permits the owner to collect any remaining Ether in the contract.

#### Overview
1. **SPDX-License-Identifier**: This line specifies the license under which the code is released. In this case, it's using the MIT License.
```
// SPDX-License-Identifier: MIT
```

2. **Solidity Version: pragma solidity ^0.6.0;** specifies that the code is intended to be compiled with Solidity version 0.6.0 or higher.
```
pragma solidity ^0.6.0;

```
4. **Import Statements**: The contract imports the SafeMath library from the OpenZeppelin Contracts library. SafeMath is used for performing arithmetic operations on unsigned integers safely to prevent overflows and underflows.
```
import 'openzeppelin-contracts-06/math/SafeMath.sol';

```

6. **State Variables:**

   - **using SafeMath for uint256;:** This line indicates that the SafeMath library will be used for safe arithmetic operations on uint256 types.

   - **mapping (address => uint) allocations;**: This mapping stores the allocation of Ether for each address.

   - **address payable public owner;**: This variable stores the address of the owner of the contract, and it's declared as payable, which means it can receive Ether.
  
```
  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;
```

7. **Constructor Fal1out():** The constructor function is named Fal1out (note that the "L" in "Fal1out" is actually a numeral "1"). It is intended to be a constructor but has a different name, which is unusual. It initializes the contract by setting the owner as the address that deploys the contract (msg.sender) and allocates an initial amount of Ether to the owner.
```
  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }
```
9. **Modifier onlyOwner:** This is a custom modifier that restricts access to certain functions to only the contract owner. If a function is marked with this modifier, it can only be called by the owner.
```
  modifier onlyOwner {
	        require(
	            msg.sender == owner,
	            "caller is not the owner"
	        );
	        _;
	    }
```
   
#### Functions
1. **allocate() Function:** This function allows any address to allocate Ether to the contract. It increases the allocation for the caller's address by the amount of Ether sent with the transaction.
```
  function allocate() public payable {
    allocations[msg.sender] = allocations[msg.sender].add(msg.value);
  }
```

2. **sendAllocation(address payable allocator) Function:** This function allows an allocator (address) to withdraw their allocated Ether from the contract. It checks that the allocator has an allocation greater than 0 and then transfers that amount of Ether to the allocator's address.
```
  function sendAllocation(address payable allocator) public {
    require(allocations[allocator] > 0);
    allocator.transfer(allocations[allocator]);
  }
```
3. **collectAllocations() Function:** Only the owner can call this function. It allows the owner to collect all the remaining Ether held by the contract. This can be used to withdraw any remaining funds in the contract to the owner's address.
```
  function collectAllocations() public onlyOwner {
    msg.sender.transfer(address(this).balance);
  }
```

4. **allocatorBalance(address allocator):** This function allows anyone to check the balance allocated to a specific allocator (address) in the contract. It returns the amount of Ether allocated to the specified address.

```
  function allocatorBalance(address allocator) public view returns (uint) {
    return allocations[allocator];
  }
```
#### Summary
### Exploit


## Solution
