# Mundo Upgrade Governance Proposal

This proposal is to perform a chain upgrade to the ixo-4 from v0.16.2 to v0.19.x at block height 7029293, which is estimated to occur on **Thursday, December 8th, UTC 14:00.**
Block times have high variance, so please monitor the chain for more precise time estimates.

## Motivation

The proposed upgrade introduces new custom modules, enhanced chain services and protocol improvements. It also includes the patch to a potential security vulnerability in the Cosmos-SDK. 

This is a high-level summary of the upgrade:

- Bumps cosmos-sdk to v0.45.9 and ibc-go v3.3.0 with full support for state sync and Interchain Accounts. **
- Removes legacy REST endpoints for broadcast & encode, in favour of full gRPC/RPC and Protobuf.
- Adds the CosmWasm Module for deploying smart contracts (requiring governance approval).
- Adds new custom ixo go-modules for Interchain Identifiers (IID), Entities and Tokens that implement the Interchain Identifiers specification.
- Introduces new features and improvements in the custom ixo go-modules for Bonds, Projects and Payments.
- Adds Tendermint standard SECP256k1 as a verification method that will now be used as the default for all the ixo custom modules, whilst maintaining the legacy ED255219 method for backward compatibility.
- Changes the chain namespace from `impacthub` to `ixo`, so that this better aligns with the syntax of the Cosmos DID Method, Interchain Identifiers and Cosmos Accounts.

## Timing

The upgrade will be scheduled to take place at block heigh 7029293, which is estimated to be on December 8, 2022 at 14:00 UTC. As block times are highly variable, please keep monitoring the expected timing and check in regularly on the ixo Validators Discord channel as this date approaches.

## Upgrade Process

When the network reaches the halt block height, the state machine stops producing new blocks. The upgrade is anticipated to take approx 30 minutes, during which time, there will not be any on-chain activity on the network. 

Upgrading requires all validators and node operators to manually substitute the existing state machine binary with the new binary. Validators are advised to use Cosmovisor to swap the binaries automatically. Instructions to migrate to use Cosmovisor: [https://docs.cosmos.network/main/tooling/cosmovisor](https://docs.cosmos.network/main/tooling/cosmovisor)

Instructions to prepare the v0.19.x binary can be found at [https://github.com/ixofoundation/genesis/tree/main/ixo-4](https://github.com/ixofoundation/genesis/tree/main/ixo-4) and the chain release is available at [https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.19.0](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v0.19.0)

## Impacts

- Client applications using the current [@ixo/client-sdk](https://www.npmjs.com/package/@ixo/client-sdk) must upgrade to [@ixo/impactxclient-sdk](https://www.npmjs.com/package/@ixo/impactxclient-sdk).
- Dependent services, such as block explorers and wallets, that don’t rely on the Cosmos Chain Name Registry must update the chain-name settings to `ixo-4`.

## Risks

Although extensive testing and simulations have been performed this upgrade could experience problems that are introduced by unknown bugs or errors not yet identified, or that appear in the context of a full network upgrade. If serious network problems are encountered, all validators must stop operating the network immediately. Core Contributors will coordinate through the `#validators`channel of the [ixo Network Discord]([https://discord.com/channels/745033220596301995/1001820078695796807](https://discord.com/channels/745033220596301995/1001820078695796807)) a contingency plan to either replace the upgrade with a new binary that includes code fixes, or abort the upgrade and revert back to the previous release of v0.16.2.

In the event of an issue at upgrade time, we should coordinate via the validators channel in Discord to come to a quick emergency consensus and mitigate any further issues.

---

# ixo Chain Upgrade Proposal

The ixo application blockchain is built using the Cosmos-SDK framework. This chain forms a core part of the digital hyper-structure for a multi-network Internet of Impact.

Since the last chain upgrade in August 2021, there have been major updates and new releases of the core Cosmos-SDK. The ixo protocols have further evolved through experience and usage, as well as through formalising core specifications through consensus processes. The technical architectures and state models for identifiers, entities, verifiable data, and tokens have been updated to comply with new standards, such as Interchain Identifiers and the Cosmos DID Method.

Significant new features and improvements have been added to all the ixo custom modules, as part of major software updates across the entire ixo stack. This document provided additional details about these changes, to support the governance proposal for this chain upgrade.

## 1. Change in Chain Namespace

The **Impact Hub** chain name convention (currently impacthub-3) will be replaced using **ixo** as the chain name prefix, whilst maintaining the sequential version that denotes the chain generation. The chain-name `ixo` will be used, with the chain-id `ixo-4`.

### Rationale

The topologies of Cosmos Internet of Blockchains networks have evolved, driven by the adoption of new protocols such as the Inter-blockchain Communication (IBC) and inter-chain security. We now are seeing less of a hub-and-spoke network model, as originally described in the Cosmos White Paper, and the emergence of more deeply interconnected network models, as described in the recent Cosmos 2.0 white paper, that includes concepts such as consumer chains, mesh security, and more innovations to come.

These developments support the ixo vision of a multi-network Internet of Impact, as described in the Internet of Impact White Paper. With the proposed upgrade of the Internet of Impact Hub main-net, we have the opportunity to better position the ixo name-space as one of many Internet of Impact infrastructures, that can continue to evolve and specialise its application-specific features and services for the Impact Economy.

The ixo protocol implements W3C standard decentralised identifiers (DIDs) as a core primitive that defines the the chain name-space with the URI prefix `did:ixo:` based on the Interchain Identifiers (IID) specification and ixo DID Method. The Cosmos account Bech-32 prefix for this network is already `ixo`.

### Potential consequences

**Neutral:** The impacts on dependant services of changing the chain-name to `ixo-4`  rather than `impacthub-4` are the same. No changes are required in the denom of the `IXO` token, or in the Bech-32 account address prefix.

**Positive:** Using `ixo` for this chain namespace will be ontologically and syntactically more correct and should reduce historical inconsistencies in how the chain name has been publicly represented in chain name registries, block explorers related and services such as Map of Zones. Using `ixo` for the chain name will be consistent with the `did:ixo` DID Method, Interchain Identifiers, and how Cosmos Account addresses are already represented.

**Negative:** There is some possibility of the brand identity of `ixo` to be misunderstood in relation to the concept of `The Internet of Impact` hyper-structure, which can be mitigated through clear communications.

## 2. Upgraded handling of cryptographic signing

All ixo custom modules will now enable message signing with Tendermint standard SECP-256k1 keys, as well as legacy ED25519 keys. 

An account address must now also be specified in all ixo custom module messages. This change in the message struct aligns the ixo blockchain with the Tendermint standards for message handling and enables new Cosmos-SDK functionality such as feegrant, authz and multisig transactions to be used by the custom ixo modules.

### Rationale

The legacy ixo modules that were deployed in the first ixo blockchain release at the end of 2018 were architected to be compatible with the emerging decentralised identifier specifications and software libraries at the time, which were predominantly biased towards using ED25519 cryptography that was considered more efficient for signing large data objects such as DID Documents, Verifiable Claims, and Verifiable Credentials. Implementing this standard across the ixo stack made sense for interoperability with the leading decentralised identity projects and software utilities. 

Tendermint validates messages signed using the ED25519 verification method. However, Tendermint SECP-256k1 is the default standard for Cosmos-SDK transactions and most of the protocols and software tools within the Cosmos ecosystem, such as wallets, are designed for this standard.

With the implementation of a new DID registry module (the IID Module) as part of the ixo chain upgrade, it will now be possible to associate multiple Verification Methods with a DID, in a way that conforms with the W3C DID-Core standard. The ixo application stack, including the ixo Impact-X Client SDK, Cellnode data stores, and client applications, have now been updated to use SECP-256k1 as the default Verification Method for cryptographic Authentication, Attestation (signing Claims and Credentials), Capability Delegation and Capability Invocation. 

### Potential consequences

**Neutral:** Both SECP256k1 and ED25519 keys will now be supported, with backward compatibility.

**Positive:** The interoperability of the ixo chain is extended to enable standard Cosmos wallets to be used for signing messages. Applications, such as Self-sovereign Digital Identity, that use ED25519 for authentication or other verification relationships, may be easier to integrate with the chain. The ixo client applications are now fully compatible with a broader ecosystem of software products and services, in keeping with the mission to build a highly interoperable, multi-chain Internet of Impact hyper-structure that will support a large ecosystem of products and services. 

**Negative:** Cosmos Accounts created with legacy ED25519 keys will have to use compatible wallets, such as the ixo Keysafe Chrome browser extension that will no longer be supported. The option of users moving their assets from legacy accounts to new accounts requires un-delegating staked assets and transfer of these assets to alternative compatible accounts.  Supporting two cryptographic schemes makes the chain more complex to maintain and use, even though this is necessary for backwards compatibility.

## 3. Upgrade to Cosmos SDK v45.9

This chain upgrade from 0.42 to 0.45.9 includes the new features, modules and security patches of the core Cosmos SDK. All legacy REST interfaces for encoding and broadcasting messages have been removed and replaced with gRPC for the chain to become fully compatible with the latest Cosmos-SDK Protobuf specification. 

Additional CosmosSDK modules introduced in this upgrade include the FeeGrant, Authz and Upgrade modules. The CosmosSDK NFT Module and Groups Module are excluded from this upgrade.

The critical DragonBerry security patch is included.

## Addition of the Cosmwasm Module

The Cosmwasm Module is added with the functionality to execute WebAssembly smart contracts. 

Deployment of CosmWasm smart contracts will require proposals to be approved through governance, as a security measure and to align new chain functionality with requirements for the Internet of Impact.

### Smart Contracts

As part of the network upgrade, deployment of the first set of Smart Contracts will be simultaneously authorised.

This includes:

- cw721 (NFT Contract with metadata)
- ixo1155
- Group Contract

## Removed the ixo `DID Module`

The ixo DID module has been deprecated and replaced by the IID module. Some of the DID module state has been migrated over in a restructured format that is compatible with IIDs, for backward compatibility. Redundancies will be removed in future releases, once all entities have migrated to the IID standard DIDs.

## New `IID Module`

The legacy ixo `DID Module` is replaced by the `IID Module` (Interchain Identifiers Module). 

The IID Module serves as the Decentralised Identifier Registry for the ixo namespace `did:ixo` and as a Trusted Data Registry and DID Resolver for DID Documents that conform to the Interchain Identifier DID Method and W3C DID-Core specification.

The Interchain Identifiers Specification describes a DID Method purposefully designed for identifying and interacting with on-chain digital assets and resources that are located in the decentralised registries and ledgers of different chain namespaces in the Internet of Blockchains. This is also the basis of the `did:cosmos` DID Method. 

## New `Entities Module`

The `Entities Module` handles the creation, resolution, updating and deactivation of Entities for Internet of Impact applications. This includes standard Entity Types such as DAOs, Projects, Oracles, Assets, Investments and Protocols, as well as any other application-specific types that users may specify.

The Entities Module interfaces programmatically with the IID Module Identifier Registry, the Projects Module Verified Data Registry, and CosmWasm Module smart contracts.

The module automatically creates an ERC721-compatible NFT for each new entity and it handles transfers of ownership and control over these assets.

This is tightly integrated with the new `IID Module`. 

## New `Tokens Module`

The `Tokens Module` handles minting of tokens that are identified using the IID Module, and minted using CosmWasm Smart Contracts, such as CW20, CW721 and ixo1155.

Minting functions are authorised using an Object Capability approach that enables the controller/s of a Token Class to delegate authorizations for accounts to call the Minter functions on a specific Smart Contracts and mint an allowed amount of tokens with a given identifier and token metadata URI.

## Upgraded Projects Module

The Projects Module is now deeply integrated with the Entities Module and uses the services of the IID Module for storing entity DID Documents, with much more granular control over creating and updating the properties of these documents.

For backwards compatibility, this upgrade includes a mapping of all existing entities to the new IID Module. This mainly affects entities historically created on the ixo Test Zone and should have no negative consequences on existing entities.

Improvements have been made to the business logic that restricted payouts and how state is managed for entities created using the Projects Module. It is now possible to make payouts to service providers, without being restricted by the state of the entity.

## Upgraded Bonds Module

The AlphaBond functions of the Bonds Module are significantly updated with a more efficient bonding curve algorithm. Interfaces have been improved for administrating and transacting with AlphaBonds, and for updating Alpha.

## Upgraded Payments Module

The business logic for handling Payment Contracts and Oracle Fee distributions have been updated to enable partial payments for use-cases such as Instalment Payments and variable amount Reimbursement Claims.

## Interchain Accounts

Allows accounts on another ICA enabled chain to carry out transactions on the ixo network and vice versa.
