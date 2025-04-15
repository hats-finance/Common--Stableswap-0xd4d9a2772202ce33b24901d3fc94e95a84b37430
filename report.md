# **Common: Stableswap Audit Competition on Hats.finance** 


## Introduction to Hats.finance


Hats.finance builds autonomous security infrastructure for integration with major DeFi protocols to secure users' assets. 
It aims to be the decentralized choice for Web3 security, offering proactive security mechanisms like decentralized audit competitions and bug bounties. 
The protocol facilitates audit competitions to quickly secure smart contracts by having auditors compete, thereby reducing auditing costs and accelerating submissions. 
This aligns with their mission of fostering a robust, secure, and scalable Web3 ecosystem through decentralized security solutions​.

## About Hats Audit Competition


Hats Audit Competitions offer a unique and decentralized approach to enhancing the security of web3 projects. Leveraging the large collective expertise of hundreds of skilled auditors, these competitions foster a proactive bug hunting environment to fortify projects before their launch. Unlike traditional security assessments, Hats Audit Competitions operate on a time-based and results-driven model, ensuring that only successful auditors are rewarded for their contributions. This pay-for-results ethos not only allocates budgets more efficiently by paying exclusively for identified vulnerabilities but also retains funds if no issues are discovered. With a streamlined evaluation process, Hats prioritizes quality over quantity by rewarding the first submitter of a vulnerability, thus eliminating duplicate efforts and attracting top talent in web3 auditing. The process embodies Hats Finance's commitment to reducing fees, maintaining project control, and promoting high-quality security assessments, setting a new standard for decentralized security in the web3 space​​.

## Common: Stableswap Overview

The contract implements stableswap invariant AMM based on Curve stableswap model.

## Competition Details


- Type: A public audit competition hosted by Common: Stableswap
- Duration: 2 weeks
- Maximum Reward: $29,984.85
- Submissions: 39
- Total Payout: $11,694.09 distributed among 3 participants.

## Scope of Audit

## Project overview

The contract implements stableswap invariant AMM based on Curve stableswap model. The contract implementation is extended to support tokens with rate oracles.

The smart contract is implemented in ink! smart contract language and adapted to work on Substrate platform.

## Audit competition scope

```
|-- common-amm-stable-swap
    |-- amm
        |-- contracts
            |-- stable_pool
		    |-- lib.rs
		    |-- token_rate.rs
        |-- traits
            |-- lib.rs
		|-- ownable2step.rs
		|-- stable_pool.rs
		|-- rate_provied.rs
    |-- helpers
        |-- stable_swap_math
            |-- fees.rs
		|-- mod.rs
        |-- constants.rs
        |-- ensure.rs
        |-- lib.rs
        |-- math.rs
```

## Medium severity issues


- **Function `force_update_rate` can reset expiration timer without updating stale price**

  A vulnerability in the `force_update_rate` function allows any user to forcefully update the price rate and manipulate the system. This function updates the timestamp `last_token_rate_update_ts` to the current time regardless of whether the price has changed. This scenario can be exploited as follows: if a token's Oracle becomes stale, a malicious user (Bob) can continually call `force_update_rate`, resetting the expiration timer without price changes. Consequently, when the token returns to an unstale state, Bob can execute a swap using the stale price before it accurately updates. This flaw enables manipulation of rates, impacting users who may unknowingly suffer from this behavior. Recommendations include preventing `last_token_rate_update_ts` from updating when the price remains unchanged to mitigate such exploits.


  **Link**: [Issue #27](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/27)


- **Potential Vulnerability in Adjustment of Amplification Coefficient in Stableswap Pools**

  Stableswap pools use an amplification coefficient (A) to adjust liquidity concentration and maintain the stablecoin peg. This coefficient can be altered by an admin using the `set_amp_coef` function to account for varying liquidity needs or changes in the stablecoin peg. However, improper changes to A, especially downward adjustments, can expose the pool to significant losses. 

An attacker might exploit this by detecting an admin's change to A, using a flashloan to imbalance the pool before the change, and then reversing the swap to profit, causing a loss to the Automated Market Maker (AMM) token inventory. The recommended solution is to implement a gradual adjustment mechanism for A, similar to what's done in StableSwap Curve contracts.


  **Link**: [Issue #39](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/39)

## Low severity issues


- **Unchecked Amounts in Stable Pool Liquidity Operations Allow Zero Value Transactions**

  In the stable pool, when adding or removing liquidity, the input vector's length is checked against the token length, but the amounts are not validated. This oversight allows transactions with zero amounts to pass and emit events, potentially causing front-end application issues due to event spamming. The amounts should be validated.


  **Link**: [Issue #37](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/37)



## Conclusion

The audit for Common: Stableswap hosted by Hats.finance concluded with three participants uncovering various vulnerabilities, earning a total payout of $11,694.09 from a maximum reward of $29,984.85. Two medium severity issues were identified. The first vulnerability pertains to the `force_update_rate` function, which allows abuse by updating the expiration timer without changing the stale price, potentially leading to rate manipulation. The second issue concerns the improper adjustment of the amplification coefficient (A) by admins, which might be exploited via flashloans to cause substantial losses to the Automated Market Maker (AMM). A solution for this includes implementing a gradual adjustment mechanism for A. Additionally, a low severity issue was identified, revealing that zero-value transactions could occur during stable pool liquidity operations due to unchecked amounts, potentially resulting in event spamming. Overall, the audit highlights the effectiveness of decentralized audit competitions in identifying and addressing critical security flaws promptly.

## Disclaimer


This report does not assert that the audited contracts are completely secure. Continuous review and comprehensive testing are advised before deploying critical smart contracts.


The Common: Stableswap audit competition illustrates the collaborative effort in identifying and rectifying potential vulnerabilities, enhancing the overall security and functionality of the platform.


Hats.finance does not provide any guarantee or warranty regarding the security of this project. Smart contract software should be used at the sole risk and responsibility of users.

