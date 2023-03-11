# ixo Mainnet Upgrade Proposal v0.20.0

*<add to the proposal message - start>*

# ixo Mainnet Upgrade Proposal v0.20.0

This proposal is to perform a Mainnet chain upgrade to ixo-4 from v0.19.3 to v0.20.0 at block height 1254500, which is estimated to occur on **Fri, 17 Mar 2023 06:30:00 UTC**.

Here is a handy way to keep track of the date/time. [https://www.mintscan.io/ixo/blocks/1254500](https://www.mintscan.io/ixo/blocks/1254500)

Block times have high variance, so please monitor the chain closely.

## Motivation

The proposed upgrade streamlines custom modules and upgrades dependencies.

This is a high-level summary of the upgrade:

- Dependent versions upgraded to:\
    - cosmos-sdk v0.45.12
    - ibc-go v4.3.0
    - wasmd v0.30.0
    - tendermint v0.34.24
- Introduces new features and improvements to the custom ixo go-modules; specifically for IID, Entity, Token, and Claims. Project and Payment have been removed.

## Upgrade Process

When the network reaches the halt block height, the state machine stops producing new blocks. The upgrade is anticipated to take approx 30 minutes, during which time, there will not be any on-chain activity on the network.

Upgrading requires all validators and node operators to manually substitute the existing state machine binary with the new binary. Validators are advised to use Cosmovisor to swap the binaries automatically. Instructions to migrate to use Cosmovisor: [https://docs.cosmos.network/main/tooling/cosmovisor](https://docs.cosmos.network/main/tooling/cosmovisor)

Instructions to prepare the v0.20.0 binary can be found at [https://github.com/ixofoundation/genesis/tree/main/ixo-4](https://github.com/ixofoundation/genesis/tree/ixo-4) and the chain release will be available at [https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0)

Testnet release candidate is available at [https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0-rc.1](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0-rc.1)

## Impacts

Client applications must upgrade to the latest version of [@ixo/impactxclient-sdk](https://www.npmjs.com/package/@ixo/impactxclient-sdk).

## Risks

Although extensive testing and simulations have been performed, this upgrade could experience problems that are introduced by unknown bugs or errors not yet identified, or that appear in the context of a full network upgrade. If serious network problems are encountered, all validators must stop operating the network immediately. Core Contributors will coordinate through the `#validators` channel of the [ixo Network Discord](https://discord.com/channels/745033220596301995/860574536779694111).

## Voting

By voting YES on this proposal, you agree that the  ixo-4 upgrade from v0.19.3 to v0.20.0 should proceed.

By voting NO on this proposal, you disagree that the ixo-4 upgrade from v0.19.3 to v0.20.0 should proceed.

By voting NO WITH VETO, you disagree with the ixo-4 upgrade from v0.19.3 to v0.20.0 taking place, and additionally wish to see depositors penalized.

By voting ABSTAIN, you decline to give an opinion on the upgrade.