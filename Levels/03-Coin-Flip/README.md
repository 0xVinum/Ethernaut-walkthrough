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
This is a coin flipping game where you need to build up your winning streak by guessing the outcome of a coin flip. To complete this level you'll need to use your psychic abilities to guess the correct outcome 10 times in a row.
## Understanding the contract
### Code
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlip {

  uint256 public consecutiveWins;
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor() {
    consecutiveWins = 0;
  }

  function flip(bool _guess) public returns (bool) {
    uint256 blockValue = uint256(blockhash(block.number - 1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;

    if (side == _guess) {
      consecutiveWins++;
      return true;
    } else {
      consecutiveWins = 0;
      return false;
    }
  }
}
```
