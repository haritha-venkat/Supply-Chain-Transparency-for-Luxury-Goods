Experiment 3: Supply Chain Transparency for Luxury Goods
Aim:
To develop a smart contract that tracks the supply chain of luxury goods, ensuring authenticity.
Algorithm:
1. The manufacturer records product creation details on-chain.
2. The product moves through different supply chain checkpoints.
3. The ownership of the product can be transferred securely.
4. Buyers can verify the product’s authenticity.
Code:
```python
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LuxurySupplyChain {
    struct Product {
        string name;
        address currentOwner;
        bool verified; // Note: This is simply set to true on registration in this version
    }
    mapping(uint256 => Product) public products;
    event ProductRegistered(uint256 productId, string name);
    event OwnershipTransferred(uint256 productId, address newOwner);

    function registerProduct(uint256 productId, string memory name) public {
        // Checks if a product with this ID has already been registered
        // (by seeing if the currentOwner is the default zero address)
        require(products[productId].currentOwner == address(0), "Product already registered");

        // Creates the new product entry, setting the caller (msg.sender) as the owner
        products[productId] = Product(name, msg.sender, true);
        emit ProductRegistered(productId, name);
    }

    function transferOwnership(uint256 productId, address newOwner) public {
        // Ensures only the current owner can transfer the product
        require(products[productId].currentOwner == msg.sender, "Not the owner");

        // Updates the owner address
        products[productId].currentOwner = newOwner;
        emit OwnershipTransferred(productId, newOwner);
    }

    function verifyProduct(uint256 productId) public view returns (string memory, address, bool) {
        // Retrieves the product details from the mapping
        Product memory p = products[productId];
        // Returns the name, current owner, and the 'verified' flag
        return (p.name, p.currentOwner, p.verified);
    }
}
```
Expected Output:
● A luxury good (e.g., a Rolex watch) is registered on-chain.
● Ownership is transferred at every checkpoint.
● Buyers can check the authenticity before purchasing.
High-Level Overview:
● Helps prevent counterfeit luxury goods.
● Teaches real-world supply chain use cases.
