# The Technical Review Process

After gaining a comprehensive understanding of the business logic behind smart contracts, security researchers embark on the critical phase of the audit: the technical review. This stage employs a blend of automated tools and manual inspection techniques to uncover potential vulnerabilities that could compromise the contract's security and integrity. Here's an in-depth look at the technical review process, detailing the methodologies and tools that researchers utilize to identify and assess vulnerabilities within smart contracts.

## Methodologies

### Call Graphs Analysis

Creating call graphs for individual contracts and the entire system provides a visual representation of all possible interactions and function calls within the contract ecosystem. These graphs help identify complex interactions, potential reentrancy points, and unexpected pathways that could be exploited.

### Static Analysis with Slither

Slither, a Solidity static analysis framework, is instrumental in detecting vulnerabilities, coding issues, and optimization opportunities. By analyzing the contract's bytecode or source code, Slither can uncover a wide array of potential security flaws without executing the contract. Security researchers leverage Slither to automate the detection of common vulnerabilities and to streamline the initial phase of the technical review.

### Code Notation with @audit Tags

Utilizing `@audit` tags (such as `@audit-info`, `@audit-ok`, `@audit-issue`) within the code comments allows researchers to systematically annotate their findings. This practice helps in organizing observations, categorizing the severity of issues, and facilitating communication with the development team regarding specific points of concern.

### Flow Diagram Creation

Developing flow diagrams for the expected interaction of contracts offers a clear visualization of the contract's operational flow, including value transfers, function calls, and state changes. These diagrams aid in understanding the contract's logic and identifying deviations from the intended behavior.

### Access Control and Upgrade Mechanisms Review

A thorough examination of access control mechanisms ensures that only authorized entities can execute sensitive functions. Researchers verify that these controls align with the contract's architecture and the libraries or standards it utilizes. Similarly, the implementation of upgrade mechanisms must be scrutinized to ensure they do not introduce vulnerabilities or provide unauthorized upgrade capabilities.

### Public and External Functions Scrutiny

Functions accessible from outside the contract (`public` and `external`) are prime candidates for review, as they can be invoked by anyone, including potential attackers. Special attention is given to functions interacting with external contracts or protocols, as these interactions pose significant risks if not correctly secured.

### Ether Handling and Contract Interactions

The review process includes assessing the contract's behavior when receiving Ether outside of standard operations, such as through `transfer` or unexpected sources like `selfdestruct`. 

The correct implementation of standards (e.g., ERC20, ERC721) must also be verified, along with the correct use of secure design patterns.

### Identifying Anti-Patterns, Logical Errors and Common Vulnerabilities

Security researchers look for anti-patterns in the codebase which can introduce vulnerabilities and weaken the contract's security posture. We identified some of these areas in the previous section on [anti-patterns](../4/10/1-anti-patterns.md). However, these were not exhaustive and there are many more to consider. We will cover some of the most important types of exploits in the following sections.  A couple of examples of the types of heuristics that can be used to identify vulnerabilities, we will cover some of the most important types of exploits in the following sections.

- **Forced ETH Receipt**: The possibility of the contract being forced to receive ETH (making it `payable`) via mechanisms like `selfdestruct` sends, and the implications on the contract's balance are explored.
  
- **Variable Double Tracking**: Identifying instances where the same information is redundantly stored in multiple locations, potentially leading to inconsistencies and vulnerabilities.

We will cover more of these and how to develop your own list of heuristics in the following sections.

### Dynamic Testing, Functional Testing, Fuzzing and POCs

As part of identifying vulnerabilities a Security Researcher may employ all variety of methods we have learned about in earlier sections including Dynamic testing, functional testing, and fuzzing, as part of technical review process. These techniques involve executing the contract in a controlled environment, simulating various scenarios, and testing the contract's behavior under different conditions. Researchers will also develop proof-of-concept (POC) exploits to validate the presence of vulnerabilities and to demonstrate their potential impact.

## Conclusion

The technical review phase of a smart contract audit is a meticulous process that combines automated tooling with manual expertise to uncover vulnerabilities. By employing strategies such as call graph analysis, static analysis with tools like Slither, and careful code annotation, security researchers can systematically identify and categorize potential security issues. Through the creation of flow diagrams and a detailed review of access controls, upgrade mechanisms, and function visibility, researchers ensure a thorough examination of the contract's security posture. This comprehensive approach enables the identification of vulnerabilities, from incorrect implementation of standards to critical security flaws, ensuring the development of secure and resilient smart contracts.