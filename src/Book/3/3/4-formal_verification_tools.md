# SMT Solvers and Formal Verification Tools

SMT Solvers and Formal Verification Tools are used to verify the correctness of smart contracts. They use formal verification to provide a higher degree of assurance about the behavior of smart contracts, ensuring they meet specified requirements.

## SMT Solvers

Satifiability Modulo Theories (SMT) solvers are automated theorem provers that can verify the correctness of smart contracts. Z3 and CVC4 are two popular SMT solvers that can be used to verify the correctness of smart contracts. The latest versions of the Solidity Compiler (solc) include support for SMT solvers.

## Formal Verification Tools

Formal verification tools are crucial in smart contract development for providing mathematical proofs of contract behavior:

- **K Framework**: K is a framework that allows you to define, or implement, the formal semantics in an intuitive and modular way. In smart contracts, it's used for verifying contract logic against specified requirements.

- **Certora**: This tool focuses on verifying the correctness of smart contracts. Certora uses formal verification to provide a higher degree of assurance about the behavior of smart contracts, ensuring they meet specified requirements.

- **VerX**: VerX is a formal verification tool that uses bounded model checking to verify the correctness of smart contracts. This novel abstraction technique allows VerX to verify the correctness of smart contracts with a high degree of assurance. It is created by ChainSecurity AG and is available as a service.

These tools are instrumental in providing a mathematical guarantee that smart contracts function as intended, adding a critical layer of security and reliability.