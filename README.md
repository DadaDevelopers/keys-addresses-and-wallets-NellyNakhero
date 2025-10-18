[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/vhoKWTLf)
# assignment-2

Generate legacy addresses, bech32 addresses and bech32m addresses

Test Evidence(Screenshots)
<img width="767" height="272" alt="Screenshot 2025-10-14 at 9 06 10 AM" src="https://github.com/user-attachments/assets/bd4f2495-ee76-4b5c-9228-97762358266d" />


What is the difference between hardened and non hardened keys

The difference between hardened and non-hardened keys lies in their derivation process and security implications.

* **Non-Hardened Keys**

   A non-hardened child key is derived from the parent key and the chain’s code using the formulae
   
   ```bash
   k(i) = k_par + hash(K_par, c_par, i)
   ```
   where:
   `k_par` = parent private key

   `K_par` = parent public key

   `c_par` = chain code

   `i` = index

   This allows the derivation of child public keys from an extended public key (xPub), which is crucial for creating watch-only wallets.

   **Hardened Keys**

   A hardened child key is derived using the parent’s private key and chain code using the formulae

   ```bash
   k(i) = k_par + hash(k_par, c_par, i)
   ```
   This method ensures that the parent’s private key cannot be derived from the child’s public key, which creates a security firewall that prevents compromise of the entire key hierarchy if the descendant private key is exposed. Hardened keys are typically used for internal wallet operations such as generating a change address, where the parent key is already secured and not exposed to external systems.


* **security comparison**

  Non-hardened keys get compromised if an attacker gains access to an extended public key and single non-hardened keys derived from it, since with this, the attacker can reverse engineer the parent’s private keys and subsequently derive all other keys in the hierarchy. As such, non-hardened keys are recommended for use cases requiring watch-only functionality, while hardened keys are preferred when the parent's private key is present and the risk of key exposure is higher.

* **indexing comparison**
   
  **Non-Hardened**: Indices from `0` to `2³¹ - 1`
  **Hardened**: Indices from `2³¹` to `2³² - 1`

* Feature Comparison

| Feature                                   | Non-Hardened                    | Hardened                          |
| ----------------------------------------- | ------------------------------- | --------------------------------- |
| Derive child from public key?             | Yes (using `xPub`)              |  No (requires private key)        |
| Secure if `xPub` + child private exposed? | No (master key can be leaked)   | Yes (master key remains secure)   |
| Example derivation path                   | `m/0/1/2`                       | `m/0'/1'/2'`                      |


Why should a wallet developer prefer deterministic wallets over non deterministic wallets
* Deterministic wallets generate keys from a single seed, which allows the entire wallet to be backed up with just one piece of information(mnemonic phrase), this eliminates the need to backup every private key which is major burden in non-deterministic wallets where each key is randomly generated and independent.

* Deterministic wallets, particularly hierarchical deterministic wallets defined by BIP-32, offer a tree-like structure that enables better organization and privacy. This structure enables developers to create different branches for different purposes without compromising security, which is not feasible with non-deterministic wallets, which lack any inherent relationship between keys.

* HD wallets allow the generation of an unlimited number of public addresses on demand without requiring private keys. This is crucial for privacy as it enables the use of a new address for every transaction, preventing address reuse, reducing the risk of linking a transaction to a single user. However, non-deterministic wallets require users to back up and manage each new key, making frequent address reuse easier due to operational burden.

* Deterministic wallets are interoperable across different wallet implementations due to standardized protocols like BIP-39(mnemonic phrases) and BIP-44(multi-account structures), which allow users to easily migrate their funds between wallets. This flexibility and standardization are absent in non-deterministic wallets, which are incompatible and harder to restore.

All these advantages in ease of backup, organizational flexibility, enhanced privacy, and cross-wallet compatibility are the main contributing factors to preferring deterministic wallets over non-deterministic wallets.
