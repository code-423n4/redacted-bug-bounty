# ✨ So you want to run a bug bounty

This `README.md` contains a set of checklists for our collaboration.

Bug Bounties use two repos:

- **a _bug bounty_ repo** (this one), which is used for scoping your bug bounty and for providing information to wardens
- **a _submissions_ repo**, where issues are submitted

Ultimately, when we launch the bug bounty, this repo will be made public and will contain links to the in-scope files to be reviewed and all the information needed for bounty participants.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the Code Blue sponsor (⭐️)**.

---

# Repo setup

## ⭐️ Sponsor: Edit this `README.md` file

- [ ] Modify the contents of this `README.md` file. Describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2022-08-foundation#readme))
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.

## ⭐️ Sponsor: Final touches

- [ ] Check that images and other files used in this README have been uploaded to the repo as a file and then linked in the README using absolute path (e.g. `https://github.com/code-423n4/yourrepo-url/filepath.png`)
- [ ] Ensure that _all_ links and image/file paths in this README use absolute paths, not relative paths
- [ ] Check that all README information is in markdown format (HTML does not render on Code4rena.com)
- [ ] Remove any part of this template that's not relevant to the final version of the README (e.g. instructions in brackets and italic)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Redacted Code Blue Details

- Max Bounty: $500,000 worth of WETH
  - Max Critical Bounty: $500,000 worth of WETH
  - Max High Bounty: $30,000 worth of WETH
  - Other Bounties: Up to $10,000 worth of WETH based on severity
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/YYYY-MM-AuditName/submit) - TODO (need submission form link which may follow a different format)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens) - TODO (need bug bounty guidelines)
- Starts December 15, 2023 20:00 UTC

❗ _Note for participants: The sponsor's repo, scope definition, and contents herein are all subject to change._

## Publicly Known Issues

- **Centralization Risks**: Some methods (such as `emergencyWithdraw`) are only accessible by the Redacted DAO multisig, which is the sole owner of the contracts. This is acceptable as the multisig is controlled by the Redacted DAO, which is a decentralized organization. These methods would only be used for emergency purposes, such as in the event of a critical bug or a hack.

- **ERC-1155 Mint Re-entrancy**: The contract is not vulnerable to reentrancy attacks because the contract does not hold any funds and does not call any external contracts in the same transaction as the mint call. The contract is also not vulnerable to re-entrancy attacks because the contract does not use any state variables that can be modified by an external contract in the same transaction as the mint call.

- **Deposit Front Running**: This issue is mitigated by pushing all validators into queue via access control once they have an effective balance of 1 ETH, meaning they have already been registered with the canonical beacon chain deposit contract.

- **Allowances and `OPERATOR_ROLE`**: The `OPERATOR_ROLE` is able to set allowances for `pxETH`. This role _only_ given to the `PirexETH` contract and is used to facilitate fee distribution. The `OPERATOR_ROLE` is not given to any external contracts or accounts.

- **Effects of `setContract` on State**: Changing withdrawal credentials aka the `rewardRecipient` contract address could corrupt state. For example, if there are initialised validators and `rewardRecipient` is changed via `setContract`, then functions like `getInitializedValidatorAt` may return incorrect withdrawal credentials. This is mitigated by the fact that `setContract` can ony be called by the owner (Redacted DAO multisig) which does extensive due diligence before executing any transactions, and `rewardRecipient` is not expected to change.

> Note: We have acknowledged all findings in referenced Audits and have either fixed them or have mitigated them. These functions are required for the protocol to work as intended.

# Pirex ETH Overview

Pirex ETH is built on top of the Redacted DAO’s Pirex platform and forms the foundation of the Dinero protocol. It is a two-token system built around ETH staking, consisting of pxETH and apxETH, tailored for different user preferences. This design gives users a choice: pxETH for liquidity or apxETH for boosted ETH staking yield.

### `pxETH` and `apxETH`

When depositing ETH, users can choose between holding pxETH or depositing to an auto compounding rewards vault for apxETH.

- **pxETH** is for those willing to forgo staking yield for liquidity. When users choose to hold pxETH, they’re opting to hold an ETH-pegged asset that can take advantage of opportunities throughout DeFi. These include providing liquidity, participating in lending protocols, and more. The Redacted DAO will be using its treasury and BTRFLY incentives to expand such opportunities for pxETH holders.

- **apxETH** is for users focused on maximizing their staking yields. After minting pxETH, users can deposit to Dinero's auto-compounding rewards vault to enjoy boosted staking yields without the hassle of running their own validators. Since some users will choose to hold pxETH, each apxETH benefits from staking rewards from more than one staked ETH, amplifying the yield for apxETH users.

### Deposits and the ETH Buffer

Most ETH deposited into the Dinero protocol via Pirex ETH is staked on the Ethereum network. However, a small fraction remains in an "ETH buffer" instead of being staked. This buffer facilitates smooth staking and unstaking, allows faster ETH withdrawals when it has funds, and will support self-contained meta transactions through the Redacted Relayer RPC in the future.

### Withdrawals

There are limits on the rate at which validators can enter and exit the Ethereum network, based on the total number of validators. Therefore, if there is a significant ETH unstaking queue, this can hamper the timeliness of ETH withdrawals from the Dinero protocol from the spinning down of validators. In these circumstances, an incentivized withdrawal pool can be used to improve pxETH liquidity from ETH unstaking.

Users can deposit ETH into a pool and receive rewards whilst they provide liquidity to that pool. Where there is an unstaking queue and ETH from the spinning down of validators is not readily available, ETH from this pool is provided to users in exchange for pxETH, with the exchange rate or price being determined by demand for ETH from the pool. As pxETH is redeemed and validators are spun down, ETH is replenished in the pool. Depositors into the withdrawal pool therefore receive rewards in exchange for potential ETH illiquidity.

As pricing depends on the demand for ETH in the pool, rewards on deposited ETH increase during periods of high demand, allowing the pool to scale when demand is high. This makes liquidity provision more efficient and cost effective.

### Yield Stripping (Coming Soon)

Yield from apxETH can be tokenized via yield stripping. For example, if a user wants to tokenize 1 year of yield for 1 pxETH deposited in the rewards vault, they can exchange 1 pxETH for:

- 1 pxETH principal semi-fungible token which can be exchanged for 1 pxETH in one year; and
- 1 pxETH yield semi-fungible token for each rewards period in the next year, which can be exchanged for the rewards earned by 1 pxETH in the rewards vault in one rewards period.

Users decide how many reward cycles they tokenize. These tokens can be used throughout DeFi and are tradable. Yield stripping provides users the ability to leverage, hedge, and speculate on future pxETH price and future yield.

## Links

<!-- @0xKubko @0xhafa please review -->

- **Previous audits:**
  - [Spearbit - PirexETH](https://github.com/redacted-cartel/audits/blob/master/dinero-pirex-eth/pirex-eth/spearbit.pdf) ([@spearbitdao](https://twitter.com/spearbitdao))
  - [Pashov - PirexETH](https://github.com/redacted-cartel/audits/blob/master/dinero-pirex-eth/pirex-eth/pashov.pdf) ([@pashovkrum](https://twitter.com/pashovkrum))
- **Documentation:**
  - [PirexETH & Dinero Documentation](https://dineroismoney.com/docs)
  - [PirexETH Whitepaper](https://dineroismoney.com/whitepaper)
  - [Dinero Litepaper](https://github.com/redacted-cartel/dinero-litepaper)
- **Website:** https://dineroismoney.com
- **Twitter:** ([@redactedcartel](https://twitter.com/redactedcartel))
- **Discord:** https://discord.gg/redactedcartel

# Scope

<!-- @0xKubko @0xhafa please populate contracts/descriptions -->
<!-- contract links are pointing to old public files, sloc is correct, files need to be updated on the public repo -->

[ ⭐️ SPONSORS: add scoping and technical details here ]

- [ ] In the table format shown below, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each _For line of code counts, we recommend running prettier with a 100-character line length, and using [cloc](https://github.com/AlDanial/cloc)._
  - [ ] external contracts called in each
  - [ ] libraries used in each

_List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus._

| Contract                                | SLOC | Purpose                                                                                                                                                                                                                                             | Libraries used                                                                                                                          |
|-----------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| [src/AutoPxEth.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/AutoPxEth.sol)                   | 489  | This contract enables autocompounding for pxETH assets and includes various fee mechanisms.                                                                                                                                                         | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts), [solmate](https://github.com/transmissions11/solmate) |
| [src/DineroERC20.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/DineroERC20.sol)                 | 77   | A Standard ERC20 token with minting and burning with access control.                                                                                                                                                                                | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts), [solmate](https://github.com/transmissions11/solmate) |
| [src/OracleAdapter.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/OracleAdapter.sol)               | 112  | This contract facilitates interactions between PirexEth, the reward recipient, and oracles for managing validators.                                                                                                                                 | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)                                                        |
| [src/PirexEth.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/PirexEth.sol)                    | 552  | This contract manages various interactions with pxETH, such as deposits, redemptions, and fee adjustments.                                                                                                                                          | [solmate](https://github.com/transmissions11/solmate)                                                                                   |
| [src/PirexEthValidators.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/PirexEthValidators.sol)          | 1063 | This contract includes functionality for handling validator-related operations and deposits.                                                                                                                                                        | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts), [solmate](https://github.com/transmissions11/solmate) |
| [src/PirexFees.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/PirexFees.sol)                   | 86   | This contract manages the distribution of protocol fees to assigned recipient.                                                                                                                                                                      | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts), [solmate](https://github.com/transmissions11/solmate) |
| [src/PxEth.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/PxEth.sol)                       | 51   | This contract manages the PxEth token, the main token for the PirexEth system used in the Dinero ecosystem. It extends the DineroERC20 contract and includes additional functionality.                                                              | None                                                                                                                                    |
| [src/RewardRecipient.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/RewardRecipient.sol)             | 158  | This contract manages rewards for validators and handles associated functionalities.                                                                                                                                                                | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/interfaces/IDepositContract.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/interfaces/IDepositContract.sol) | 24   | This is the Ethereum 2.0 deposit contract interface.                                                                                                                                                                                                | None                                                                                                                                    |
| [src/interfaces/IOracleAdapter.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/interfaces/IOracleAdapter.sol)   | 18   | This interface defines the methods for interacting with OracleAdapter.                                                                                                                                                                              | None                                                                                                                                    |
| [src/interfaces/IPirexEth.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/interfaces/IPirexEth.sol)        | 61   | This interface defines the methods for interacting with PirexEth.                                                                                                                                                                                   | None                                                                                                                                    |
| [src/interfaces/IPirexFees.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/interfaces/IPirexFees.sol)       | 23   | This interface defines functions related to the distribution of fees in the Pirex protocol.                                                                                                                                                         | None                                                                                                                                    |
| [src/interfaces/IRewardRecipient.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/interfaces/IRewardRecipient.sol) | 40   | This interface defines functions related to dissolving and slashing validators in the Pirex protocol.                                                                                                                                               | None                                                                                                                                    |
| [src/libraries/DataTypes.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/libraries/DataTypes.sol)         | 87   | This library provides data structures and enums crucial for the functionality of the Pirex protocol.                                                                                                                                                | None                                                                                                                                    |
| [src/libraries/Errors.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/libraries/Errors.sol)            | 204  | This interface defines errors that might occur in the PirexEth system.                                                                                                                                                                              | None                                                                                                                                    |
| [src/libraries/ValidatorQueue.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/libraries/ValidatorQueue.sol)    | 347  | This library provides functions for adding, swapping, and removing validators in the validator queue. It also includes functions for popping validators from the end of the queue, retrieving validator information, and clearing the entire queue. | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/tokens/UpxEth.sol](https://github.com/redacted-cartel/pirex-eth-contracts/blob/master/src/tokens/UpxEth.sol)       | 127  | This is a semi-fungible ERC1155 token contract with minting and burning capabilities, using AccessControl for role-based access.                                                                                                                    | [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts), [solmate](https://github.com/transmissions11/solmate) |

## Out of scope

<!-- @drahrealm @dhruvinparikh @0xKubko please review -->

Contracts:

- `ChainlinkFunctionsOracleAdapter.sol`

Vendor Libraries:

- `chainlink`
- `solidty-cborutils`
- `@ensdomains/buffer`

# Additional Context

- Trusted Roles

  - `DEFAULT_ADMIN_ROLE`: Set external contract addresses
  - `GOVERNANCE_ROLE`: Manages validator queue, set fee params, pause deposits to beacon chain deposit contract
  - `KEEPER_ROLE`: Harvest rewards, update status when a validator is slashed and top up validator stake when active balance goes below effective balance
  - `OPERATOR_ROLE`: Allows `PirexETH` to perform specific internal actions (eg. approve allowances for fee distribution)
  - `ORACLE_ROLE`: Updates the state of the validator when it is dissolved
  - `MINTER_ROLE`: Allows `PirexETH` to mint `pxETH` and `apxETH`
  - `BURNER_ROLE`: Allows `PirexETH` to burn `pxETH` and `apxETH`

- EIP Specifications:

  - `pxETH`: Should comply with `ERC-20` standard
  - `apxEth`: Should comply with `ERC-4626` standard
  - `upxETH` (Unlocking) and `RFN` (Yield Stripping): Should comply with `ERC-1155` standard

- In the event of DOS, we would consider a finding to be valid if it is reproducible for a minimum duration of 4 hours.

## Attack ideas (Where to look for bugs)

<!-- cc: @drahrealm @dhruvinparikh @0xhafa -->

- Where funds are entering/exiting protocol
  - TODO: Add links to relevant contracts

## Main invariants

- Setting and updating contract addresses (`pxETH` address, etc) which are controlled by the `DEFAULT_ADMIN_ROLE`
- `pxETH` and `apxETH` are minted and burned in a 1:1 ratio
- `outstandingRedemptions`
- `pendingWithdrawal`
- `pendingDeposits`

## Scoping Details

<!-- @0xKubko @0xhafa please complete -->

[ ⭐️ SPONSORS: please confirm/edit the information below. ]

<!-- ND: should we open source contracts -->

```
- If you have a public code repo, please share it here: https://github.com/redacted-cartel/pirex-eth-contracts
- How many contracts are in scope?:
- Total SLoC for these contracts?:
- How many external imports are there?:
- How many separate interfaces and struct definitions are there for the contracts within scope?:
- Does most of your code generally use composition or inheritance?: Inheritance
- How many external calls?: 1 - Beacon Chain Deposit Contract
- What is the overall line coverage percentage provided by your tests?:
- Is this an upgrade of an existing system?: No
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.): ERC-20, ERC-1155, ERC-4626
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:
- Please describe required context:
- Does it use an oracle?: Yes - Chainlink for triggering dissolve validator
- Does it use a side-chain?: No
- Describe any specific areas you would like addressed:
```

# Tests

<!-- cc: @drahrealm @dhruvinparikh -->

### Foundry

- Install dependencies `npm i`
- Install forge thru Foundry (see: https://github.com/foundry-rs/foundry)
- Make sure to fill out `scripts/loadEnv.sh` with the correct env variables as seen in `loadEnv.example.sh`
- Run the test with `scripts/forgeTest.sh`

### Slither

We don’t actually run slither locally but it’s part of the CI thru Github Action. For slither related findings/reports, all of them are known and part of the design (like sending ether to arbitrary address, which is required for the protocol to work) (edited)
