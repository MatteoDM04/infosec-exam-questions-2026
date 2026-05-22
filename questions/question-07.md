# Question 7

Blockchain consensus relies on the assumption that the longest available chain is the correct one.

**Explain the longest-chain rule used in blockchains such as Bitcoin.**

**How do forks arise, and how are they resolved?**

**Discuss under which assumptions this rule is secure, and what happens if these assumptions no longer hold.**

## Answer

Blockchain consensus relies on the fundamental assumption that the longest available chain is the correct one. Below is an explanation of how this rule operates in networks like Bitcoin, how forks are handled, and the security assumptions that keep the system stable.

### The Longest-Chain Rule
In Bitcoin, there is no Trusted Third Party (TTP) to decide which transactions are valid. Instead, nodes follow the chain that has the most cumulative **Proof-of-Work (PoW)**. 

If a node receives two conflicting versions of the blockchain, it must always choose to work on the one that is longer (the one with the most blocks). This is known as the **Longest-Chain Rule**. By enforcing this rule, all independent nodes across the network eventually converge and agree on the exact same history of transactions.

### Forks
*   **How Forks Arise:** A fork occurs when two different miners solve the PoW puzzle at roughly the same time. Both miners broadcast their valid blocks simultaneously. As a result, part of the network sees "Block A" first, while another part sees "Block B" first, temporarily splitting the blockchain into two competing branches.
*   **How Forks Are Resolved:** Forks are resolved naturally through the longest-chain rule when the next block is mined. Nodes will mine on top of the block they received first, but they keep the alternative branch in memory just in case. As soon as a new block is successfully added to one of the branches, that branch becomes the longest chain. The network immediately recognizes it as the valid path, and the shorter, abandoned branch is discarded (its blocks become "orphaned").

### Security Assumptions

Bitcoin's validation and consensus mechanisms rely on strict security assumptions:

*   **Honest Majority of CPU Power:** The system assumes that more than 50% of the network's total computational (hashing) power is controlled by honest nodes dedicated to growing the legitimate chain. Because the honest majority possesses more power, their chain will naturally grow faster than any malicious chain, making it mathematically impossible for an attacker to catch up and rewrite history.
*   **What Happens if the Assumption Fails?** If a malicious actor gains control of more than 50% of the network's power (a **51% Attack**), the network risks **Double Spending**. The attacker can secretly mine an alternative branch of blocks while transacting normally on the public chain. Because they have the majority of the power, their secret chain will eventually grow longer than the public one. When the attacker finally broadcasts this secret chain, the network is forced by the longest-chain rule to accept it, effectively rewriting history and erasing the original transactions.

> **Note on Alternative Consensus:** While Bitcoin relies on computational power via Proof-of-Work, other networks like Ethereum have transitioned to **Proof-of-Stake (PoS)**, where security is maintained by validators staking native cryptocurrency rather than running hardware rigs.

**Source**: Slides Blockchain 12-13 + 18-24 + Gemini
