# Scope of Audits

The scope of Web3 security audits distinctly covers both on-chain and off-chain components, each with its unique set of challenges and requirements. 

- **On-Chain Components**: This primarily involves the smart contract code that is deployed on the blockchain. Audits here focus on the contract's logic, data handling, security patterns, and compliance with best practices to prevent vulnerabilities like re-entrancy, overflow/underflow, and improper access controls. The immutable nature of blockchain makes it crucial to thoroughly audit these components before deployment.

- **Off-Chain Components**: These include the application's backend systems, APIs, front-end interfaces, and any other off-chain infrastructure that interacts with the blockchain. While these components can be updated or fixed with fewer constraints than on-chain code, they play a crucial role in maintaining the overall security posture of Web3 projects. Audits examine how these off-chain elements interact with smart contracts and the blockchain, ensuring secure data transmission, authentication, and access controls.

Differentiating between these components is critical because it dictates the audit methodology, tools, and strategies to be employed. While on-chain audits require a deep understanding of smart contract languages and the blockchain platform, off-chain audits leverage more traditional security assessment techniques.