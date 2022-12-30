pragma solidity ^0.6.12;

import "https://github.com/Gabrielcdecarvalho/volvoinu/blob/main/volvoinu.sol";

contract VolvoInu is BEP3 {
    // Set the name, symbol, decimals, and total supply for the token
    string public name = "Volvo inu";
    string public symbol = "VIU";
    uint8 public decimals = 18;
    uint256 public totalSupply = 10000000 * (10 ** uint256(decimals));

    // Set the owner wallet address
    address public owner = 0x1ADc23b58f82587386495934C8Fd707Fab313a79;

    // Set the buy and sell taxes
    uint public buyTax = 9;
    uint public sellTax = 9;

    // Implement the BEP3 functions for buying and selling tokens
    function buy(uint256 amount) public {
        // Calculate the cost of the tokens including the buy tax
        uint256 cost = amount.mul(1 + buyTax / 100);

        // Check that the caller has enough balance to cover the cost
        require(msg.value >= cost, "Insufficient balance");

        // Transfer the tokens to the caller
        _transfer(msg.sender, amount);

        // Send the cost minus the buy tax to the owner
        owner.transfer(cost.mul(100 - buyTax).div(100));
    }

    function sell(uint256 amount) public {
        // Check that the caller has enough tokens to sell
        require(_balanceOf(msg.sender) >= amount, "Insufficient balance");

        // Calculate the price of the tokens including the sell tax
        uint256 price = amount.mul(1 - sellTax / 100);

        // Transfer the tokens from the caller
        _transfer(msg.sender, amount);

        // Send the price to the caller
        msg.sender.transfer(price);
    }
}
