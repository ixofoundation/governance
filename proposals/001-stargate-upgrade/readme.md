# IMPACT HUB Upgrade to `impacthub-3`

## 1. Key Results
1. The Impact Hub validators commit to upgrade the network using the ixo-blockchain software version `impacthub-v1.6.0`, which implements Stargate.
2. The Impact Hub genesis parameters will be updated with the following critical changes:
  - Chain Name `impacthub-3`
    - `chain_id` value will be set to `impacthub-3`
  - Maximum validator set: `50`
    - Under `staking` module, `params.max_validators` will be set to `50`
3. No change in Inflation
  - Inflation remains at `0%` with a change in inflation delayed to a future governance vote
    - Under `mint` module, `params.inflation_max` will be left at `0.000000000000000000`
    - Under `mint` module, `params.inflation_min` will be left at `0.000000000000000000`
    - Under `mint` module, `params.inflation_rate_change` will be left at `0.000000000000000000`

## 2. Updated Software Features in v1.6.0
This is a major upgrade which includes all Cosmos Stargate features.

### Upgraded to Cosmos SDK v0.42.6 "Stargate"
- Introduced protobuf and grpc-gateway
- Deprecated use of ixocli which is now part of ixod

### Deprecated oracles and treasury modules
- Added new demo scripts: demo_gov_param_change.sh, ibc/demo.sh
- Updated and fixed all demo scripts

### Project module new message types
- Introduced MsgUpdateProjectDoc
- Fixed bug which does not allow us to go to PAIDOUT status if there are zero funds

### Bonds module update 
(#239):
- Implements reserve withdrawals

### Other miscellaneous changes and improvements 
(#241, #242)
- Add Starport support with config.yml config file (#241)
- Add /ixo/ prefix for did module proto queries (#241)
- Updated SDK documentation
- Improved README instructions (#241)
- Added a swagger.yaml file with gRPC endpoints for ixo modules (bonds, did, payments, project) (#242)

## 3. Upgrade Method and Plan
This upgrade will be conducted manually by coordination between the validators. 

## Upgrade Time

The mainnet upgrade is expected to take place on 2021 August 19 at 12:00 UTC.
- Epoch time: 1629331200
- Block height: 2281750

## Git Commit

The git commit of [impacthub v1.6.0](https://github.com/ixofoundation/ixo-blockchain/releases/tag/v1.6.0) is:

`21e2c962e18220888d529bf156406260a321cf80`

## 4. Migration Guides
### For Node Operators:
This software upgrade is compulsory, if approved by governance.

[This upgrade guide](https://github.com/ixofoundation/genesis/blob/impacthub-3/impacthub-3/README_UPGRADE.md) provides further instructions.

### For Application Developers:
This mainnet upgrade includes the legacy REST interfaces, so developers do not necessarily need to migrate the on-chain node interface for applications.
However, developers are encouraged to start migrating client applications to the new Stargate [grpc](https://grpc.io/) interface standard.

### For Users:
For Impact Hub users, it's always recommended to safely backup your wallet mnemonics or keystore files.

## Exceptions

There are some circumstance where the proposal should be abandoned even if it passes:

A critical vulnerability may be found in the software. If the development teams change their recommended version of impacthub, the validator set should implicitly abandon this upgrade procedure. A future proposal will be made for the Hub to upgrade to the new target commit.
