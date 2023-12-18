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

# Redacted Code Blue details

- Max Bounty: $500,000 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/YYYY-MM-AuditName/submit) - TODO (need submission form link which may follow a different format)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens) - TODO (need bug bounty guidelines)
- Starts December 15, 2023 20:00 UTC

❗ _Note for participants:  The sponsor's repo, scope definition, and contents herein are all subject to change._

## Publicly Known Issues

[ ⭐️ SPONSORS: Are there any known issues or risks deemed acceptable that shouldn't lead to a valid finding? If so, list them here. ]

# Pirex ETH Overview

Pirex ETH is built on top of the Redacted DAO’s Pirex platform and forms the foundation of the Dinero protocol. It is a two-token system built around ETH staking, consisting of pxETH and apxETH, tailored for different user preferences. This design gives users a choice: pxETH for liquidity or apxETH for boosted ETH staking yield.

### `pxETH` and `apxETH`

When depositing ETH, users can choose between holding pxETH or depositing to an auto compounding rewards vault for apxETH.

pxETH is for those willing to forgo staking yield for liquidity. When users choose to hold pxETH, they’re opting to hold an ETH-pegged asset that can take advantage of opportunities throughout DeFi. These include providing liquidity, participating in lending protocols, and more. The Redacted DAO will be using its treasury and BTRFLY incentives to expand such opportunities for pxETH holders.

apxETH is for users focused on maximizing their staking yields. After minting pxETH, users can deposit to Dinero's auto-compounding rewards vault to enjoy boosted staking yields without the hassle of running their own validators. Since some users will choose to hold pxETH, each apxETH benefits from staking rewards from more than one staked ETH, amplifying the yield for apxETH users.

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

- **Previous audits:**
  - [Spearbit - PirexETH](https://github.com/redacted-cartel/audits/blob/master/dinero-pirex-eth/pirex-eth/spearbit.pdf) ([@spearbitdao](https://twitter.com/spearbitdao))
  - [Pashov - PirexETH](https://github.com/redacted-cartel/audits/blob/master/dinero-pirex-eth/pirex-eth/pashov.pdf) ([@pashovkrum](https://twitter.com/pashovkrum))
- **Documentation:**
  - [PirexETH & Dinero Documentaion](https://dineroismoney.com/docs)
  - [PirexETH Whitepaper](https://dineroismoney.com/whitepaper)
  - [Dinero Litepaper](https://github.com/redacted-cartel/dinero-litepaper)
- **Website:** https://dineroismoney.com
- **Twitter:** ([@redactedcartel](https://twitter.com/redactedcartel))
- **Discord:** https://discord.gg/redactedcartel

# Scope

[ ⭐️ SPONSORS: add scoping and technical details here ]

- [ ] In the table format shown below, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each _For line of code counts, we recommend running prettier with a 100-character line length, and using [cloc](https://github.com/AlDanial/cloc)._
  - [ ] external contracts called in each
  - [ ] libraries used in each

_List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus._

| Contract                                                                                                | SLOC | Purpose                | Libraries used                                           |
| ------------------------------------------------------------------------------------------------------- | ---- | ---------------------- | -------------------------------------------------------- |
| [contracts/folder/sample.sol](https://github.com/code-423n4/repo-name/blob/contracts/folder/sample.sol) | 123  | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

## Out of scope

_List any files/contracts that are out of scope for this bounty._

# Additional Context

- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Please list specific ERC20 that your protocol is anticipated to interact with. Could be "any" (literally anything, fee on transfer tokens, ERC777 tokens and so forth) or a list of tokens you envision using on launch.
- [ ] Please list specific ERC721 that your protocol is anticipated to interact with.
- [ ] Which blockchains will this code be deployed to, and are considered in scope for this bounty?
- [ ] Please list all trusted roles (e.g. operators, slashers, pausers, etc.), the privileges they hold, and any conditions under which privilege escalation is expected/allowable
- [ ] In the event of a DOS, could you outline a minimum duration after which you would consider a finding to be valid? This question is asked in the context of most systems' capacity to handle DoS attacks gracefully for a certain period.
- [ ] Is any part of your implementation intended to conform to any EIP's? If yes, please list the contracts in this format:
  - `Contract1`: Should comply with `ERC/EIPX`
  - `Contract2`: Should comply with `ERC/EIPY`

## Attack ideas (Where to look for bugs)

_List specific areas to address - see [this blog post](https://medium.com/code4rena/the-security-council-elections-within-the-arbitrum-dao-a-comprehensive-guide-aa6d001aae60#9adb) for an example_

## Main invariants

_Describe the project's main invariants (properties that should NEVER EVER be broken)._

## Scoping Details

[ ⭐️ SPONSORS: please confirm/edit the information below. ]

```
- If you have a public code repo, please share it here:
- How many contracts are in scope?:
- Total SLoC for these contracts?:
- How many external imports are there?:
- How many separate interfaces and struct definitions are there for the contracts within scope?:
- Does most of your code generally use composition or inheritance?:
- How many external calls?:
- What is the overall line coverage percentage provided by your tests?:
- Is this an upgrade of an existing system?:
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.):
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:
- Please describe required context:
- Does it use an oracle?:
- Describe any novel or unique curve logic or mathematical models your code uses:
- Is this either a fork of or an alternate implementation of another project?:
- Does it use a side-chain?:
- Describe any specific areas you would like addressed:
```

# Tests

_Provide every step required to build the project from a fresh git clone, as well as steps to run the tests with a gas report._

_Note: Many wardens run Slither as a first pass for testing. Please document any known errors with no workaround._
