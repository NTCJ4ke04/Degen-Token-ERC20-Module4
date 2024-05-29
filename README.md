# Degen-Token-ERC20-Module4

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
import "./ownable.sol";

contract DegenToken is ERC20, Ownable {
    mapping(uint256 => uint256) public Gun;

    event GunRedeemed(address indexed player, uint256 indexed GunId, uint256 amount);

    constructor(uint256 initialSupply) ERC20("Degen", "DGN") {
        _mint(msg.sender, initialSupply * 10 ** 0);
        Gun[45] = 6000;   // Gun 1: Glock 45 - Price: 6000 Coins
        Gun[47] = 8000;   // Gun 2: Ak-47 - Price: 8000 Coins
        Gun[16] = 10000;  // Gun 3: M16 - Price: 10000 Coins
        Gun[24] = 20000;  // Gun 4: M24 - Price: 20000 Coins
    }

    function mint(address account, uint256 amount) public {
        _mint(account, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    function setItemPrice(uint256 itemId, uint256 price) external onlyOwner {
        Gun[itemId] = price;
    }

    function redeemGun(uint256 Id) public {
        // Ensure item price is set and player has enough balance
        require(Gun[Id] > 0, "Item price not set");
        require(balanceOf(msg.sender) >= Gun[Id], "Insufficient balance");

        // Transfer tokens from player to owner (in-game store)
        _transfer(msg.sender, jakeOwner, Gun[Id]);
        emit GunRedeemed(msg.sender, Id, Gun[Id]);
    }
}
