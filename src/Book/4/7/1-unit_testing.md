# Unit Testing

We covered Unit Testing in the context of Solidity smart contracts in a [section 3.4.1](../../3/4/1-unit_testing.md). 

An security researcher performing an audit can utilize existing Unit Tests in multiple way. First, to understand the contract's behavior and to identify potential vulnerabilities. The gaps in Unit Testing are also of use, particularly for functionality that has security implications like access control or cross-contract interactions, as this can be a red flag for potential vulnerabilities. 

When inspecting a smart contract, auditors should start by identifying the most likely areas of concern by making multiple passes through the code. Once this is complete a part of digging into these potential bugs is to review the existing Unit Tests to understand the contract's behavior and identify potential vulnerabilities.

Lastly, these Unit Tests and their scaffolding, the setup of variables and environment can assist in building POCs as well as stateless and stateful (property, invariant) fuzz testing of the contract.