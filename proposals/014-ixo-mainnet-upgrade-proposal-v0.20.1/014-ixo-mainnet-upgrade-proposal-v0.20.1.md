# ixo Mainnet Upgrade Proposal v0.20.0

This proposal is to perform a Mainnet chain upgrade with the following changes:
- Upgrade v0.19.3 to v0.20.0 at block height 1345000, which is estimated to occur on **Thu Mar 23 2023 09:54:40 UTC**.
- Change the ixo chain-id from ixo-4 to ixo-5.
- Start block height from 1.

Here is a handy way to keep track of the date/time. [https://www.mintscan.io/ixo/blocks/1345000](https://www.mintscan.io/ixo/blocks/1345000)

Block times have high variance, so please monitor the chain closely.

## Motivation

The proposed upgrade streamlines custom modules and upgrades dependencies.

This is a high-level summary of the upgrade:

- Dependent versions upgraded:
    - cosmos-sdk v0.45.12
    - ibc-go v4.3.0
    - wasmd v0.30.0
    - tendermint v0.34.24
- Custom ixo go-modules enhanced:
    - IID, Entity, Token, and Claims modules improved.
    - Project and Payment modules removed.

## Upgrade Process

When the network reaches the halt block height, the state machine stops producing new blocks. The upgrade is anticipated to take approx 30 minutes, during which time, there will not be any on-chain activity on the network.

NB: IBC Relayers should upgrade their clients during this period.

Upgrading requires all validators and node operators to manually substitute the existing state machine binary with the new binary.

Instructions to prepare the v0.20.0 binary can be found at [https://github.com/ixofoundation/genesis/tree/main/ixo-5](https://github.com/ixofoundation/genesis/tree/ixo-5) and the chain release will be available at [https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0)

Testnet release candidate is available at [https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0-rc.4](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.20.0-rc.4)

## Impacts

Client applications must upgrade to the latest version of [@ixo/impactxclient-sdk](https://www.npmjs.com/package/@ixo/impactxclient-sdk).

## Risks

Although extensive testing and simulations have been performed, this upgrade could experience problems that are introduced by unknown bugs or errors not yet identified, or that appear in the context of a full network upgrade. If serious network problems are encountered, all validators must stop operating the network immediately. Core Contributors will coordinate through the `#validators` channel of the [ixo Discord](https://discord.com/channels/745033220596301995/860574536779694111).

## Voting

By voting YES on this proposal, you agree to upgrading from v0.19.3 to v0.20.0 and changing the chain-id to ixo-5.

By voting NO on this proposal, you do not want the ixo-4 upgrade from v0.19.3 to v0.20.0 and the chain-id changing to ixo-5.

By voting NO WITH VETO, you disagree with the ixo-4 upgrade from v0.19.3 to v0.20.0 taking place and the chain-id changing to ixo-5, and additionally wish to see depositors penalized.

By voting ABSTAIN, you decline to give an opinion on the upgrade.