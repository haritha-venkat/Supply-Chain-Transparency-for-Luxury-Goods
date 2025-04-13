# Experiment 3: Supply Chain Transparency for Luxury Goods
## Aim:
To develop a smart contract that tracks the supply chain of luxury goods, ensuring authenticity.
## Algorithm:
1. The manufacturer records product creation details on-chain.
2. The product moves through different supply chain checkpoints.
3. The ownership of the product can be transferred securely.
4. Buyers can verify the product’s authenticity.
## Code:
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
## Expected Output:
### register product:
![Screenshot 2025-04-13 135601](https://github.com/user-attachments/assets/cd439e2e-c0d9-47ec-a8c8-98dbe25e5ab8)
![Screenshot 2025-04-13 135553](https://github.com/user-attachments/assets/4a840428-7545-48ec-b166-6525a117c937)

### verify product
![Screenshot 2025-04-13 135651](https://github.com/user-attachments/assets/4f3b245a-c2f1-400e-a8ac-e9e00177216e)
![Screenshot 2025-04-13 135659](https://github.com/user-attachments/assets/990f635f-8fbd-49ea-ac44-ce3a40900b88) 

### ownership transaction: 
![Screenshot 2025-04-13 135757](https://github.com/user-attachments/assets/1cfc319a-d714-4d6c-b350-a180da99215b)
![Screenshot 2025-04-13 135805](https://github.com/user-attachments/assets/8b3e1240-23f2-4757-a49c-df857d2ae363)

### re-verification:
![Screenshot 2025-04-13 135913](https://github.com/user-attachments/assets/6c76f4d8-ff9b-4360-a0c9-749a3e65518e)


 
