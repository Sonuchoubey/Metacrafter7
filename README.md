---

# Create Subnet and then Deploy it

## Overview
DeFi Empire is an exciting blockchain-based gaming experience where players can collect, build, and battle with their digital assets. By engaging in various game activities, players have the opportunity to earn rewards. This project is a DeFi Kingdom clone on the Avalanche network, merging decentralized finance and gaming to create an immersive universe.

## Prerequisites
Before you begin, ensure you have the following tools and resources:
- Unix computer (MacOS or Linux) or WSL installed on Windows
- [Remix online IDE](https://remix.ethereum.org/)
- [Metamask wallet extension](https://metamask.io/)
- Web browser
- Kali Linux (preferred)

## Project Steps

### 1. Set Up Your EVM Subnet
To create a custom EVM subnet on the Avalanche network, follow the instructions provided in the [guide](https://docs.avax.network/) and refer to the Avalanche documentation. This will allow you to deploy your smart contracts with low fees and create a custom token.

### 2. Define Your Native Currency
Create your own native currency to serve as the in-game currency for your DeFi Kingdom clone. This currency will be used for transactions, rewards, and various in-game activities.

### 3. Connect to Metamask
To connect your EVM Subnet to Metamask, follow the instructions in the provided [guide](https://docs.avax.network/build/tutorials/smart-contracts/setup-metamask). Make sure to select your custom EVM subnet as the network in Metamask.

### 4. Deploy Basic Building Blocks
To deploy foundational smart contracts for your game, you can leverage Solidity and Remix. These contracts will define activities such as battling, exploring, and trading. Example contracts are provided below.

### 5. Test Your Application
Use Remix to interact with your deployed smart contracts for your DeFi Kingdom clone. Deploy tokens, liquidity pools, and other important components of your game. Test functionalities like battling, exploring, and trading within your game.

## Smart Contracts

### ERC20 Token Contract
The contract is an implementation of a standard ERC20 token. It includes features such as transfers, allowances, minting, and burning. Players can utilize this token as an in-game currency.

**ERC20.sol**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ERC20 {
    uint public totalSupply;

    string public name = "ARYAN";
    string public symbol = "AYN";
    uint8 public decimals = 18;

    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address to, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint amount) external returns (bool) {
        allowance[from][msg.sender] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### Vault Contract
The Vault contract allows users to deposit and withdraw tokens while keeping track of the total supply and individual balances. It is used to manage liquidity within the game.

**Vault.sol**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

## Usage
To start your DeFi Kingdom adventure, follow these steps:
1. Set up your EVM subnet using the provided guide and Avalanche documentation.
2. Define your native currency within the game.
3. Connect your EVM Subnet to Metamask as outlined in the guide.
4. Deploy the foundational smart contracts using Remix.
5. Test your DeFi Kingdom clone by interacting with the deployed smart contracts through Remix.

## Author
Sonu Choubey

## License
This project is licensed under the MIT License.

---
