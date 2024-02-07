#  Smart Contract Auditing 

> Note: This section is a work in progress (WIP) and will be expanded in the near future.

## [Introduction to Web3 Auditing](1/0-intro.md)
- [Overview of Auditing](1/1-auditing_overview.md) : Definition and importance of security assessments in Web3 projects.
- [Scope of Audits](1/2-scope_of_audits.md) : Differentiating between on-chain smart contract code and off-chain components.
- [Target Audience for Audits](1/3-target_audience.md) : Understanding who benefits from the audits.
- [Expectations](1/4-expectations_limitations.md) : Understanding what audits aim to achieve and their limitations.
- [Ethical and Professional Standards in Auditing](1/5-ethical_standards.md) : The importance of ethical and professional standards in the auditing process.

## [Choices and Considerations](2/0-choices_considerations.md)
- [Differentiating Audit Types](2/1-audit_types.md) : New, repeat, fix, retainer, and incident audits.
- [Phases of the Smart Contract Audit Process](2/2-phases.md) : The stages of a typical audit process.
- [Auditing firms and Independent Auditors](2/3-firms_independents.md) : A look at the industry and the participants. 
- [Decentralized Auditing](2/4-decentralized_auditing_bug_bounties.md) : Gameified systems like Code4rena, Sherlock, Codehawks, Hats.finance 
- [Guidelines on Audit Selection](2/6-guide_audit_selection.md) : Guidelines for project teams on selecting the audit type based on a project's stage and needs.

## [Preparation and Initialization](3/0-preparation_initialization.md)
- [Audit Prerequisites](3/1-prerequisites.md) : Essential elements and documentation required before starting an audit.
- [Audit Checklist](3/2-checklist.md) : A comprehensive list to prepare projects for security audits.
- [Initial Code Walkthrough](3/3-code_walkthrough.md) : The importance of a preliminary code review before the audit begins.
- [Communication Channels](3/4-communication_channels.md) : Messaging Channels and regular meetings for updates via Video Conference are normal, there may be barriers due to languages and time zones. Ongoing communication is key to a successful audit.

## [Audit Reports](4/0-audit_reports.md)
- [Components of an Audit Report](4/1-audit_report_components.md) : Detailed explanation of what is included in audit reports.
- [Interpreting Audit Findings](4/2-findings.md) : How to understand and act on the findings presented in the report.
- [Recommendations and Remediations](4/3-recommendations.md) : Addressing and mitigating the identified issues and vulnerabilities.

## [The Basics](5/0-auditor_basics.md) 
- [Security Researcher's Toolbox](5/1-auditors_base_toolbox.md): Tools & Smart Contract Development Basics - IDEs, Plugins, AuditWizard, AI (ChatGPT), 
- [Overview of Audit Techniques](5/2-methodology.md) : The process of auditing smart contracts and the techniques used.
- [Secure Smart Contract Design](5/3-secure_smart_contract_design.md) The principles of secure smart contract design, such as minimizing attack surface, using tested and proven libraries, access control, and following security specific design pattern
- [NatSpec and Documentation](5/4-natspec_documentation.md) : The importance of documentation and the NatSpec standard for smart contracts.

## [Smart Contract Auditing Tools](6/0-auditing_tools.md)
- [Foundry Forge](6/6-foundry.md) : A Rust based Development Framework that includes many useful tools for understanding and testing smart contract including a stateless and stateful (Invariant) fuzzer
- [Mythril](6/2-mythril.md) : A security analysis tool for Ethereum smart contracts. It uses concolic analysis (dynamic symbolic execution), SMT Solving taint analysis, and control flow checking to detect a variety of security vulnerabilities.
- [Slither](6/1-slither.md) : A static analysis framework that can detect common issues such as re-entrancy, suicidal contracts, and incorrect visibility.
- [Echidna](6/3-echidna.md) : A property-based fuzzer that can be used to find bugs in smart contracts.
- [Certora](6/5-certora.md) : Formal verification tool for smart contracts.
- [MythX](6/4-mythx.md) : A SAAS security analysis platform for Ethereum smart contracts.

## [Smart Contract Testing](7/0-smart_contract_testing.md)
- [Unit Testing](7/1-unit_testing.md) : Unit tests for auditors individual components of your contract function as expected.
- [Integration Testing](7/2-integration_testing.md) : Testing multiple components of a contract together to ensure they work correctly in unison.
- [Creating POCs](7/3-creating_pocs.md) : Creating Proof of Concepts to demonstrate the vulnerabilities found in the audit.

## [Fuzzing](8/0-fuzzing.md) 
- [Stateless vs Stateful Fuzzing](8/1-stateless_stateful_fuzzing.md) : The difference between stateless and stateful fuzzing and when to use each.
- [Stateless Fuzzing with Foundry](8/2-stateless_fuzzing_foundry.md) : How to use stateless fuzzing tools such as Foundry
- [Stateful Fuzzing with Echidna](8/3-stateful_fuzzing_echidna.md) : How to use stateful fuzzing tools such as Echidna
- [Identifying Invariants in Smart Contracts](8/4-identifying_invariants.md) : How to identify invariants for stateful fuzzing in smart contracts

## [Formal Verification](9/0-formal_verification.md) 
- [Benefits and Limitations of Formal Verification](9/1-benefits_limitations.md) : Discusses the benefits and limitations of formal verification and how it can be used to improve the security of smart contracts.
- [Introduction to Formal Verification Tools](9/2-tools.md) : Introduces formal verification tools such as Certora and how they can be used to verify the correctness of smart contracts.
- [Real World Examples](9/3-real_world_applications.md) : Provides real world examples of how formal verification has been used to find and fix vulnerabilities in smart contracts.
- [Best Practices for Formal Verification](9/4-best_practices.md) : Discusses best practices for using formal verification tools and how to get the most out of them.
- [Challenges and Future Directions](9/5-challenges_future_directions.md) : Discusses the challenges in adoption and the future directions of formal verification for smart contracts.

##  [Mastering the EVM and Low-Level Programming](10/0-evm_low_level_programming.md)
- [Data Structures in the EVM](10/1-data_structures.md) : Types of data locations in the EVM, such as stack, memory, storage, and calldata
- [The Yul language and Inline Assembly](10/2-yul_inline_assembly.md) : Low-level intermediate programming for the EVM
- [Auditing inline Assembly](10/3-auditing_yul_inline_assembly.md) : How to audit smart contracts that use inline assembly and Yul
- [Calldata specifics](10/4-analyzing_calldata.md): decoding a complex call data example and how to use the abi coder library
- [The Huff Language](10/5-huff_language.md) : A brief introduction to Huff, a low-level language for the EVM that uses macros

## [Identifying Vulnerabilities](11/0-identifying_vulns.md)
- [Understanding Business Logic](11/1-business_logic.md) : Understanding the business logic and the intended interactions within and between contracts is paramount.
- [Technical Review Process](11/2-technical_review.md) : The process of identifying vulnerabilities in smart contracts.
- [Developing Heuristics](11/3-developing_heuristics.md) : Develop and utilize heuristics for auditing smart contracts.
- [Common Smart Contract Vulnerabilities](11/4-common_vulns.md) 
- [Timestamp Dependence](11/5-timestamp_dependence.md) : Smart contracts that use the `block.timestamp` variable may have this vulnerability.
- [Gas Limit and Loops](11/6-gas_related_vulnerabilties.md) : Loops that run for an indeterminate number of iterations can hit the gas limit, causing transactions to fail.
- [Denial of Service (DOS) Attacks](11/7-dos_attacks.md) : Exploiting design flaws or gas-related vulnerabilities to make contracts unusable.
- [Re-entrancy Attacks](11/) : This occurs when an external contract hijacks the control flow, and makes recursive calls to the original contract.
- Delegatecall : `delegatecall` is a low-level function similar to a dynamic library call in other languages. If not used carefully, it can lead to serious vulnerabilities.
- integer overflow/underflow,
- Discusses the importance of rounding issues and how they can affect the accuracy and security of smart contracts

##  Upgradeability Patterns and Vulnerabilities 
- Upgradeability and the security implications for smart contract development, incident response and maintenance
- [Upgrade Patterns] Compares and contrasts different upgradeability patterns, such as proxy contracts, delegate calls, and eternal storage
- Reveals some common upgradeability vulnerabilities and how to avoid them, such as storage collisions, function clashes, and malicious upgrades

##  Front-running vectors 
- Define front-running as the act of exploiting the ordering of transactions in the mempool to gain an unfair advantage
- Illustrates how front running can affect defi protocols, such as Uniswap, Curve, and Yearn
- Discusses some possible solutions and mitigations, such as using commit-reveal schemes, batching transactions, or using layer 2 solutions

##  Ethereum cryptography and signature malleability 
- Cover the basics of cryptography and how it is used in Ethereum for signing and verifying transactions and messages
- Explain the concept of signature malleability and how it can lead to replay attacks and double spending
- Shows how to prevent signature malleability using EIP-712 and EIP-191 standards

## Analyzing DeFi Security
- The risks and vulnerabilities associated with perpetuals, such as funding rate manipulation, liquidation cascades, and oracle attacks
- Types of DeFi products, such as decentralized exchanges, lending platforms, yield farming protocols, and derivatives like options and futures along with their associated risks and vulnerabilities
- A look at Uniswap V2 & V3 and how it implements concentrated liquidity and range-bound pools to understand Front-running, Back-running and sandwich attacks.
- A look at Perpetuals, which are synthetic assets that track the price of an underlying asset without expiration. The mechanics of perpetuals, such as funding rate, margin, leverage, liquidation, and settlement
- Impermanent Loss : In automated market makers like Uniswap, liquidity providers can suffer losses due to price fluctuations.
- Price Oracle Manipulation : DeFi protocols often rely on price oracles for asset prices. If these oracles are manipulated, it can lead to serious consequences.
- Flash Loan Attacks : Flash loans allow users to borrow assets and return them within the same transaction. If not handled properly, they can be used to manipulate market prices and exploit DeFi protocols.
- Exploring some advanced attacks that target specific defi protocols or features, such as ERC-4626 inflation attack, AMM arbitrage, and oracle manipulation

## Case Studies and Examples
- Detailed Analysis of notable Smart Contract Audits
- Forensics and Post-Mortem Analysis
- A look at the subject of how to Analyzing Exploits
- Analysis of notable audit cases and lessons learned.
- Learning from Historical Audits: Successes and Failures
- Analyzing Past Attacks : Analysis of several past attacks on DeFi protocols, understanding how they happened, what vulnerabilities were exploited, and how they could have been prevented.

##  Continuing Education and Resources 
- Advanced Courses and Certifications: Additional courses and certifications that can further knowledge and skills in smart contract auditing.
- Online Channels, Communities, Newsletters and Forums : Connect with other auditors, ask questions, and stay up-to-date on the latest news and trends in the field.
- Books and Publications : Key books and publications that every smart contract auditor should read.