# Unit Testing

Creating and executing unit tests is one of the primary steps in creating secure smart contracts. Here we'll take a deeper dive into the process and best practices for unit testing in smart contract development.:

## Conceptualizing Unit Tests
When conceptualizing any unit test the process begins with **identifying the specific test cases** that each function or component must pass. This encompasses ensuring normal operation, accounting for edge cases, and preparing for potential failure modes. 

For smart contracts a significant emphasis must always be placed on security aspects and unit test cases should be developed to specifically scrutinize **security vulnerabilities** such as **unauthorized access and unsafe cross-contract interactions**. 

One way to accomplish this is to use Test Driven Development (TDD) with a with a focus on security. The TDD approach involves writing tests for specific functionalities before implementing the code itself. It ensures that security considerations are integrated from the outset, rather than being retrofitted. 

By adopting TDD, developers can create a suite of unit tests that serve as a security net, checking for vulnerabilities as the contract evolves. This method fosters a culture of security-first thinking, crucial for developing robust smart contracts. Even if a strict TDD methodology is not in place it can be very helpful to understand how one will create unit tests, and other test, for security verification and consider building these out as early as possible.

## Designing Unit Tests

In designing these unit tests, the **principle of isolation** is paramount. Each test is crafted to examine individual functions or components in isolation, facilitating precise identification of any failures. The tests are organized following the **Arrange-Act-Assert (AAA) pattern**, which segments the test into setup, execution, and verification phases. This structured approach ensures a comprehensive examination of each aspect of the contract's functionality and security. 

To implement these tests, developers leverage testing frameworks that are sometimes tailored to the greater development framework while others are more open to be used with a variety of structures. **Hardhat** for example is often paired with **Brownie** which provides extensive built-in capabilities for testing smart contracts while **Foundry Forge** offers a more complete solution for building, deploying and testing. These frameworks not only simplify the testing process but also integrate advanced features that support the rigorous evaluation of smart contracts from both functionality and security perspectives.

## Programming Languages & Unit tests


In writing Solidity smart contracts, developers can leverage various programming languages alongside Solidity itself to write comprehensive and effective tests. For instance, JavaScript is widely used with frameworks like Truffle and Waffle, where the Chai assertion library becomes a staple for writing tests. Python, known for its simplicity and readability, is primarily utilized through the Brownie framework, offering a Pythonic approach to smart contract testing. Foundry Forge, on the other hand uses Solidity to run tests. 

Each test framework can generally be used to accomplish the same end result in basic unit testing. There are, significant differences between them when to how easily and robustly this can be accomplished and when it comes to fuzzing and invariant testing. So the choice is largely based on familiarity, preference, ease of use, features and the specific needs of the project. It is also possible to mix certain aspects from different frameworks although this is generally discourage due to the add risk that comes with complexity. It is better to build existing Unit Tests in a new framework than to have two running at the same project. To make the best decision on which to use one should have a practical understanding of the each and the specific requirements of the project.

With other languages, such as Rust, developers can use the Foundry Forge framework to write tests in Rust, leveraging its performance and safety features to ensure the reliability and security of smart contracts. THe release of Arbitrum Stylus in 2023 reveals the likely future for Smart Contracts and their associated tests as one in which any major language may be used in conjunction with other protocols like WASM.  This flexibility in language choice allows developers to utilize their preferred languages and tools, enhancing the efficiency and effectiveness of the testing process.

### Writing Unit Tests in Solidity

Let's create a simplified example with Solidity smart contracts to illustrate how to write a unit test for checking unsafe cross-contract interactions, particularly focusing on reentrancy attacks. We'll use two contracts: `SafeBank` (Contract A) designed to be secure against reentrancy, and `Attacker` (Contract B) attempting to exploit it. For the unit test, we'll utilize the Hardhat framework with JavaScript.

### Example 3.4.1-1: Unit Testing for Reentrancy Attack

 Contract A: SafeBank.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract SafeBank is ReentrancyGuard {
    mapping(address => uint) public balances;

    function deposit() public payable {
        require(msg.value > 0, "Deposit amount must be positive");
        balances[msg.sender] += msg.value;
    }

    function withdraw() public nonReentrant {
        uint balance = balances[msg.sender];
        require(balance > 0, "Insufficient funds");

        (bool sent, ) = msg.sender.call{value: balance}("");
        require(sent, "Failed to send Ether");

        balances[msg.sender] = 0;
    }
}
```

### Contract B: Attacker.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ISafeBank {
    function deposit() external payable;
    function withdraw() external;
}

contract Attacker {
    ISafeBank public safeBank;

    constructor(address _safeBankAddress) {
        safeBank = ISafeBank(_safeBankAddress);
    }

    // Fallback function is called when SafeBank sends Ether to this contract.
    receive() external payable {
        if (address(safeBank).balance >= 1 ether) {
            safeBank.withdraw();
        }
    }

    function attack() external payable {
        require(msg.value >= 1 ether, "Need at least 1 ether");
        safeBank.deposit{value: msg.value}();
        safeBank.withdraw();
    }

    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
```

### Unit Test: test/unsafeInteractionTest.js

Using Hardhat with JavaScript to test for the reentrancy attack:

```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("SafeBank and Attacker Interaction", function () {
  it("Should prevent reentrancy attack", async function () {
    const SafeBank = await ethers.getContractFactory("SafeBank");
    const safeBank = await SafeBank.deploy();
    await safeBank.deployed();

    const Attacker = await ethers.getContractFactory("Attacker");
    const attacker = await Attacker.deploy(safeBank.address);
    await attacker.deployed();

    // Attacker deposits 1 ether to SafeBank
    await attacker.attack({ value: ethers.utils.parseEther("1") });

    // Check balances to ensure attack was not successful
    const attackerBalance = await attacker.getBalance();
    expect(await ethers.provider.getBalance(safeBank.address)).to.equal(ethers.utils.parseEther("1"));
    expect(attackerBalance).to.be.below(ethers.utils.parseEther("1"));
  });
});
```

This test setup first deploys the `SafeBank` contract and then the `Attacker` contract, simulating an attack by depositing and attempting to withdraw Ether in a reentrant manner. The `expect` statements verify that the `SafeBank`'s balance remains unchanged and the `Attacker` cannot extract more Ether than deposited, ensuring the reentrancy guard effectively prevents the attack.

### Execution and Analysis
- **Run Tests Regularly**: Execute unit tests frequently during development to catch and fix errors early.
- **Review Test Outcomes**: Analyze failures to understand and correct defects.
- **Continuous Improvement**: Refine and add tests as the contract evolves or as new vulnerabilities are discovered.

### Security Perspective
From a security standpoint, unit testing is invaluable for ensuring that functions are not only performing as expected under typical conditions but also handling errors and malicious inputs gracefully. Tests should cover scenarios such as input validation, permission checks, and the contract's response to abnormal or unexpected inputs.

## Major Unit Testing Frameworks for Smart Contracts

### Unit Testing Frameworks for Smart Contract Development

| Framework             | Description                                                                                                      | Key Features                                                                                                                          | Benefits for Security                                                     |
|-----------------------|------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **Truffle**           | A comprehensive development environment, testing framework, and asset pipeline for Ethereum.                      | - Clean-Room Environment<br>- Minimal Tests<br>- Assertion Flexibility<br>- Ethereum Client Compatibility                            | Enables isolated and controlled testing environments for security audits. |
| **Hardhat**           | A development environment focused on developer productivity, with built-in testing and extensible task runner.   | - Extensibility<br>- Built-in Testing<br>- Scriptable Deployment                                                                      | Facilitates flexible testing and integration with security tooling.       |
| **Remix-IDE**         | An online Solidity IDE with integrated testing capabilities, ideal for quick prototyping.                        | - Integrated Testing<br>- Web-Based                                                                                                    | Simplifies testing with immediate feedback and minimal setup.             |
| **Foundry Forge**     | A fast, portable, and modular toolkit for Ethereum application development, focusing on testing and deployment.  | - Speed and Efficiency<br>- Rust-Based for Reliability<br>- Integrated with the Ethereum Ecosystem                                    | Offers high-performance testing, critical for comprehensive security audits.|

Truffle:

Description: Truffle is a versatile Ethereum Swiss Army knife. It serves as a development environment, testing framework, and asset pipeline for Ethereum.
Key Features:
Clean-Room Environment: Solidity test contracts live alongside JavaScript tests as .sol files. Truffle ensures a separate test suite per contract, maintaining a clean-room environment.
Minimal Tests: Truffle encourages minimalistic tests by avoiding the need to extend from any contract (like a Test contract). This gives you complete control over the contracts you write.
Assertion Flexibility: While Truffle provides a default assertion library, you can easily switch to your preferred one.
Ethereum Client Compatibility: Truffle allows you to run Solidity tests against any Ethereum client.

Example:

```solidity
pragma solidity >=0.4.25 <0.6.0;
import "truffle/Assert.sol";
import "truffle/DeployedAddresses.sol";
import "../contracts/MetaCoin.sol";

contract TestMetaCoin {
    function testInitialBalanceUsingDeployedContract() {
        MetaCoin meta = MetaCoin(DeployedAddresses.MetaCoin());
        uint expected = 10000;
        Assert.equal(meta.getBalance(tx.origin), expected, "Owner should have 10000 MetaCoin initially");
    }

    function testInitialBalanceWithNewMetaCoin() {
        MetaCoin meta = new MetaCoin();
        uint expected = 10000;
        Assert.equal(meta.getBalance(tx.origin), expected, "Owner should have 10000 MetaCoin initially");
    }
}
```

Output:
```bash
$ truffle test
Compiling your contracts...
...
TestMetaCoin
  ✓ testInitialBalanceUsingDeployedContract (79ms)
  ✓ testInitialBalanceWithNewMetaCoin (65ms)

Contract: MetaCoin
  ✓ should put 10000 MetaCoin in the first account (38ms)
  ✓ should call a function that depends on a linked library (42ms)
  ✓ should send coin correctly (120ms)
```

### Foundry Forge

**Description**: Foundry Forge is part of the Foundry suite, a set of tools optimized for Ethereum smart contract development. It's built with a focus on speed, efficiency, and reliability, making it a standout choice for developers prioritizing security.

**Key Features**:
- **Speed and Efficiency**: Executes tests rapidly, significantly reducing development and testing cycles.
- **Rust-Based**: Leverages Rust’s performance and safety, offering a robust testing environment.
- **Ecosystem Integration**: Seamlessly works with other Ethereum development tools and frameworks.

**Benefits for Security**:
- The high-speed execution allows for more extensive and frequent testing, ensuring thorough coverage of potential security vulnerabilities.
- Rust’s inherent safety features reduce the risk of errors in the test suite itself, enhancing the reliability of security tests.
- Being fully integrated with the Ethereum ecosystem means developers can combine Forge with other security tools for a layered security approach.

Forge's emphasis on performance and integration facilitates a rigorous and efficient testing process, essential for identifying and mitigating security risks in smart contract development.

(see Ex 3.1.3-1 for a Foundry Forge example)

### Hardhat

**Description**: Hardhat caters to Ethereum development with a focus on tasks running and productivity, offering built-in testing capabilities and extensibility through plugins.

**Key Features**:
- **Custom Task Creation**: Allows for tailored development workflows.
- **Integrated Testing Framework**: Provides tools for immediate testing.
- **Scriptable Deployments**: Enables automated, script-based contract deployments.

use Foundry Forge tools to create a unit test in that checks if at function is restricted to the contract owner, similar to functionality provided by OpenZeppelin's `Ownable` contract, you would focus on ensuring that only the owner can call certain functions. How such a test could be structured using Foundry:

Here we use Foundry Forge tools to create a unit test in that checks if at function is restricted to the contract owner, similar to functionality provided by OpenZeppelin's `Ownable` contract, you would focus on ensuring that only the owner can call certain functions. How such a test could be structured using Foundry:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "forge-std/Test.sol";
import "../src/OwnableContract.sol"; // Your contract that inherits from OpenZeppelin's Ownable

contract OwnableContractTest is Test {
    OwnableContract ownableContract;
    address nonOwner = address(0x1);

    function setUp() public {
        ownableContract = new OwnableContract(); // Assume OwnableContract uses OpenZeppelin's Ownable
        ownableContract.transferOwnership(address(this)); // Set the test contract as the owner
    }

    function testOnlyOwnerCanAccess() public {
        // Test passes if the onlyOwner function is called by the owner
        ownableContract.onlyOwnerFunction();

        // Attempt to call the function as a non-owner should revert
        vm.prank(nonOwner); // Forge's way to impersonate another address
        vm.expectRevert("Ownable: caller is not the owner"); // Specify expected revert message
        ownableContract.onlyOwnerFunction();
    }
}
```

This example demonstrates testing an `onlyOwnerFunction` from the `OwnableContract` which should only be accessible by the contract's owner. It uses Foundry's `vm.prank` to simulate a call from a non-owner address and `vm.expectRevert` to assert that the call reverts with the expected error message. This test ensures that the ownership access control is functioning as intended, providing a security check against unauthorized access.

**Benefits for Security**:
- Custom tasks can include security-specific checks.
- Immediate testing supports rapid vulnerability detection.
- Automated deployments ensure consistent and secure deployment processes.

Hardhat:

Description: Hardhat is a development environment and task runner for Ethereum that focuses on developer productivity.
Key Features:
Extensibility: Hardhat allows you to add custom tasks and plugins.
Built-in Testing: It includes a testing framework out of the box.
Scriptable Deployment: You can script deployments using JavaScript.
Use Case: Ideal for developers who want flexibility and extensibility.

Remix-IDE:

Description: Remix-IDE is an online Solidity IDE with built-in testing capabilities.
Key Features:
Integrated Testing: Remix provides an integrated testing environment.
Web-Based: No need to install anything locally; you can use it directly in your browser.
Use Case: Great for quick prototyping and testing directly in the browser.