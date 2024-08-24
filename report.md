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


- **Allowing Forceful Rate Updates Lets Users Manipulate Prices for Economic Gains**

  The core problem involves the `force_update_rate` function, which any user can call to update a token rate's timestamp without checking if the rate has actually changed. This can be exploited in the following way: if Token A's Oracle becomes stale, a malicious user (Bob) can continually call `force_update_rate` to reset the token rate's expiration timer, even though the rate remains outdated. Once the Oracle returns to a normal state, Bob can execute a trade (`swap_exact_in`) using the stale price since the system would still consider the rates current due to the manipulated timestamps. This issue can also affect honest users unintentionally. To prevent exploitation, it is recommended to avoid updating the timestamp if the price hasn't changed.


  **Link**: [Issue #27](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/27)


- **Potential Loss in Stableswap Pools Due to Changes in Amplification Coefficient**

  Stableswap pools use an amplification coefficient (A) that can be adjusted based on the stablecoins' peg and liquidity concentration needs. An admin can schedule adjustments to A using the `set_amp_coef` function, potentially exposing the pool to a loss if increased or decreased too drastically. One attack scenario involves an attacker exploiting this change by using a flash loan to imbalance the pool, timing the A change, and swapping in reverse for a profit, causing a loss to the Automated Market Maker (AMM) token inventory. The article recommends a gradual change of 0.1% per block, but this is challenging to manage manually. Implementing a ramping up method for A, similar to that in StableSwap Curve contracts, is suggested for safer operations.


  **Link**: [Issue #39](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/39)

## Low severity issues


- **Validation Missing for Amount Value in Stable Pool Liquidity Functions**

  The current implementation of liquidity addition/removal in the stable pool only checks if the input vector length matches the token length, but doesn't verify the amounts. This can lead to zero amounts passing successfully, causing potentially disruptive events in the frontend application. Validation of input amounts is needed.


  **Link**: [Issue #37](https://github.com/hats-finance/Common--Stableswap-0xd4d9a2772202ce33b24901d3fc94e95a84b37430/issues/37)



## Conclusion

In conclusion, the Hats.finance audit competition for Common: Stableswap successfully demonstrated the effectiveness of decentralized auditing. This two-week competition attracted 39 submissions, rewarded three participants with a total payout of $11,694.09 out of the maximum $29,984.85, and identified several vulnerabilities. Medium-severity issues included the ability for users to manipulate token rates via the `force_update_rate` function, potentially leading to economic gains, and the risk of destabilizing pools due to abrupt changes in the amplification coefficient. Low-severity issues were also noted, such as the lack of input amount validation in liquidity functions, which could cause front-end disruptions. These findings underline the importance of such audit competitions in uncovering potential vulnerabilities proactively, ensuring robust, secure DeFi protocols. Overall, the event underscored Hats.finance's commitment to decentralized, cost-efficient, and high-quality security assessments, setting new standards for Web3 security.

## Disclaimer


This report does not assert that the audited contracts are completely secure. Continuous review and comprehensive testing are advised before deploying critical smart contracts.


The Common: Stableswap audit competition illustrates the collaborative effort in identifying and rectifying potential vulnerabilities, enhancing the overall security and functionality of the platform.


Hats.finance does not provide any guarantee or warranty regarding the security of this project. Smart contract software should be used at the sole risk and responsibility of users.

