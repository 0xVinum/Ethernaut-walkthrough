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

The "Fallout" smart contract serves as an example of a basic Ethereum contract for allocating and distributing Ether. It demonstrates the use of the SafeMath library for secure arithmetic operations and showcases the implementation of a custom modifier to manage access control.

While this contract provides essential functionality, it has some unconventional naming and structure issues.
In summary, this smart contract allows an owner to receive and allocate Ether among multiple addresses. Allocators can increase their allocation by calling the allocate() function, and they can withdraw their allocated Ether using the sendAllocation() function. The owner can collect all the remaining Ether in the contract using collectAllocations(). The contract also provides a way to check the allocated balance of any address. However, there are some issues and unconventional naming in the code (such as the constructor having a different name than "constructor"), which might require attention if this contract is intended for production use.





### Exploit
Upon inspecting the code, it becomes evident that there is a typographical error present. The contract's name is correctly defined as "Fallout," but the constructor function is mistakenly named "Fal1out," featuring a numerical '1' in place of the intended lowercase 'l'. This typographical oversight results in the failure of the constructor function's execution during contract deployment, rendering the 'owner' variable unaltered. In essence, "Fal1out" is perceived as a regular function rather than a constructor.

The resolution for this challenge is relatively straightforward. We simply need to invoke the "Fal1out" function, which has not been previously executed.

## Solution
```
function exploit() internal override {
    vm.startPrank(player);

    // invoke the "Fal1out" function
    level2.Fal1out();

    vm.stopPrank();
}
```
