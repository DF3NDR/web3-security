# Unchecked return values

Although we already covered Re-entrancy it is important to address it further. Unchecked call return values represent a critical vulnerability class in Ethereum smart contracts, particularly affecting low-level functions like `call()` and `send()`. These functions are designed for external calls and sending Ether but inherently possess a risk: they continue execution without reverting the operation if an error occurs, merely returning `false`. This oversight can lead to vulnerabilities if developers fail to check these return values, potentially allowing malicious actors to exploit the contract. This article explores the unchecked call return values vulnerability, its real-world implications, and strategies for mitigation, aimed at security researchers and developers.

## The Vulnerability Explained

Low-level functions such as `call()` and `send()` are essential for interacting with external contracts and accounts. However, their non-reverting nature on failure necessitates explicit checks by the developer to ensure the intended behavior. Failing to verify the success of these calls can leave the contract in an inconsistent state or vulnerable to exploitation.

Consider a contract that attempts to send Ether without checking the operation's success:
```solidity
function withdrawBalance(uint256 _amount) public {
  require(balances[msg.sender] >= _amount);
  balances[msg.sender] -= _amount;
  etherLeft -= _amount;
  msg.sender.send(_amount); // Risky: No check on the return value
}
```
In this example, if the `send()` operation fails—for reasons like the recipient contract lacking a payable fallback function, the call stack depth reaching its limit, or the contract running out of gas—the `etherLeft` variable inaccurately reflects the contract's state, potentially leading to financial discrepancies.

## Historical Exploits

The unchecked call return values vulnerability has been exploited in attacks against prominent contracts such as Etherpot and King of the Ether Throne, demonstrating the real-world consequences of this oversight. An early version of the BTC Relay contract also suffered from this vulnerability, highlighting its prevalence and impact.

## Mitigation Strategies

### Using `transfer()` for Safety

A straightforward mitigation technique is to use `transfer()` instead of `send()`. The `transfer()` function automatically reverts the entire transaction if the call fails, providing a safer alternative for sending Ether:
```solidity
function withdrawBalance(uint256 _amount) public {
  require(balances[msg.sender] >= _amount);
  balances[msg.sender] -= _amount;
  etherLeft -= _amount;
  msg.sender.transfer(_amount); // Safer: Automatically reverts on failure
}
```

### Explicitly Checking Return Values

When using `send()` or other low-level calls, it's crucial to check the return value explicitly and revert the transaction if the operation fails:
```solidity
function withdrawBalance(uint256 _amount) public {
  require(balances[msg.sender] >= _amount);
  balances[msg.sender] -= _amount;
  etherLeft -= _amount;
  if (!msg.sender.send(_amount)) revert(); // Explicit check and revert on failure
}
```

### Prevention and Best Practices

- **Withdrawal Pattern**: Adopt the withdrawal pattern, separating the logic of sending Ether from the contract's main logic. This approach delegates the responsibility of handling failed transactions to the user initiating the withdrawal, enhancing security.
- **Reentrancy Guards**: Protect transactions from reentrancy attacks by employing reentrancy guards or adhering to the Checks-Effects-Interactions pattern, ensuring that external calls are made after all state updates.
- **Validate Return Values**: Always validate the return values of low-level calls (`call()`, `callcode()`, `delegatecall()`, `send()`) and revert transactions manually when necessary to maintain contract integrity.

## Conclusion

Unchecked call return values pose a significant security risk in smart contract development, with historical exploits underscoring the need for vigilance and robust mitigation strategies. By adopting safer alternatives like `transfer()`, explicitly checking return values, and employing preventive programming patterns, developers and security researchers can protect Ethereum smart contracts against this class of vulnerabilities. Ensuring the reliability and security of smart contract interactions remains paramount in the development of trustworthy and resilient decentralized applications.