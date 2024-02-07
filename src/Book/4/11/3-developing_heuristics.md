# Developing Heuristics

For security researchers focused on identifying vulnerabilities within smart contracts, developing a personalized database of heuristics is an invaluable strategy. This database not only serves as a comprehensive checklist for common and emerging security issues but also enhances the efficiency and effectiveness of the audit process. By categorizing vulnerabilities based on tags, complexity, and detailed notes, researchers can quickly reference relevant heuristics during audits, ensuring a thorough examination of contracts under review. This article outlines how security researchers can build and utilize such a database, with examples to illustrate the approach.

## Creating a Heuristic Database

The foundation of a heuristic database is the systematic categorization of vulnerabilities, each defined by specific characteristics:

1. **Tags**: Keywords or phrases that encapsulate the essence of the vulnerability, making it easier to identify related issues during an audit.
2. **Complexity**: A relative measure of how difficult it is to identify and exploit the vulnerability, aiding researchers in prioritizing their efforts.
3. **Notes**: Detailed observations, patterns to look for, potential impacts, and mitigation strategies, providing a comprehensive understanding of each vulnerability.

## Examples of Heuristics

### Reentrancy - Classic
- **Tags**: External Call
- **Complexity**: High
- **Notes**: Focus on identifying external calls (call, transfer, send, delegatecall) that could allow an attacker to reenter the same function. Verify the sequence of checks, effects, and interactions to ensure they are correctly ordered.

### Assembly Return Misses Modifiers
- **Tags**: Low-Level Calls
- **Complexity**: Medium
- **Notes**: Review assembly code for potential bypasses of security modifiers or checks, focusing on the correct implementation and handling of low-level calls.

### Array Too Long To Delete
- **Tags**: Array
- **Complexity**: Medium
- **Notes**: Examine the use of the `delete` keyword for dynamic arrays, ensuring that operations do not lead to out-of-gas errors due to excessive length.

## Utilizing the Heuristic Database

Once established, the heuristic database becomes a dynamic tool in the security researcher's arsenal. Here are strategies for effectively using the database during smart contract audits:

1. **Pre-Audit Preparation**: Review the database to refresh knowledge on common vulnerabilities and recent additions, tailoring the audit focus based on the contract's characteristics.
2. **During the Audit**: Use tags to quickly navigate to relevant heuristics based on the contract's features or observed code patterns. This approach ensures no vulnerability category is overlooked.
3. **Post-Audit Analysis**: Update the database with new findings, refinements to existing heuristics, or adjustments based on the latest developments in smart contract security.

## Maintaining the Database

Keeping the heuristic database current is crucial for its effectiveness. Regular updates should incorporate new vulnerabilities discovered through audits, community-reported issues, and changes in smart contract development practices. Collaboration with other security professionals can also enrich the database, introducing diverse perspectives and experiences.

## Conclusion

For security researchers dedicated to uncovering vulnerabilities in smart contracts, a well-maintained database of heuristics is an indispensable resource. By systematically categorizing vulnerabilities and refining the database with ongoing learning and discoveries, researchers can enhance their audit methodologies, contribute to the security of the blockchain ecosystem, and stay ahead in the ever-evolving landscape of smart contract vulnerabilities.