# The map of the machine

Every function in this repository's Solidity, pinned to the source in this commit (1093 functions).
`python3 tools/check-map.py` proves it still matches.

Source digest: `8c94eaa7f5343e083063bd065284b9c671e8b652ab1f352c3f52679dfb1efef1`

Per-function facts live in `map/<cluster>.json`: `authority`, `authority_gate_quote`,
`reads`, `writes`, `value`, `edges` (TRUSTED/UNTRUSTED), `reachability`, `observations`.
`semantics` says how far to trust them:

- **fresh** — code unchanged since the node was mapped
- **stale** — code changed since: the facts describe older code, read the source
- **missing** — function added after mapping: read the source

Below: the **entry points** (externally callable, state-changing) per cluster, in review order.

| cluster | files | functions | fresh | stale | missing | entry points |
|---|---|---|---|---|---|---|
| hook | 9 | 148 | 138 | 7 | 3 | 59 |
| registry | 6 | 89 | 89 | 0 | 0 | 53 |
| pool | 2 | 78 | 75 | 2 | 1 | 21 |
| perp | 5 | 261 | 202 | 12 | 47 | 59 |
| rotation | 7 | 98 | 82 | 11 | 5 | 39 |
| nft | 10 | 172 | 165 | 2 | 5 | 73 |
| governance | 2 | 48 | 48 | 0 | 0 | 12 |
| seed | 5 | 69 | 69 | 0 | 0 | 16 |
| art | 6 | 61 | 61 | 0 | 0 | 7 |
| deploy | 14 | 69 | 64 | 1 | 4 | 18 |

## hook

Files: `CauldronHook.sol`, `vendor/BaseHook.sol`, `vendor/HookMiner.sol`, `cauldron/FeeRouteLib.sol`, `cauldron/DefaultFeeRouter.sol`, `cauldron/ReserveLib.sol`, `cauldron/RoyaltyRouter.sol`, `cauldron/LegacyBuyLib.sol`, `cauldron/SurtaxLib.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `BaseHook.beforeInitialize` | vendor/BaseHook.sol:36 | poolManager | NONE | fresh |
| `BaseHook.afterInitialize` | vendor/BaseHook.sol:49 | poolManager | NONE | fresh |
| `BaseHook.beforeAddLiquidity` | vendor/BaseHook.sol:62 | poolManager | NONE | fresh |
| `BaseHook.beforeRemoveLiquidity` | vendor/BaseHook.sol:80 | poolManager | NONE | fresh |
| `BaseHook.afterAddLiquidity` | vendor/BaseHook.sol:98 | poolManager | NONE | fresh |
| `BaseHook.afterRemoveLiquidity` | vendor/BaseHook.sol:121 | poolManager | NONE | fresh |
| `BaseHook.beforeSwap` | vendor/BaseHook.sol:144 | poolManager | NONE | fresh |
| `BaseHook.afterSwap` | vendor/BaseHook.sol:161 | poolManager | NONE | fresh |
| `BaseHook.beforeDonate` | vendor/BaseHook.sol:180 | poolManager | NONE | fresh |
| `BaseHook.afterDonate` | vendor/BaseHook.sol:199 | poolManager | NONE | fresh |
| `CauldronHook.setSweepFailOpen` | CauldronHook.sol:186 | ? | ? | missing |
| `CauldronHook.legacyBuyStep` | CauldronHook.sol:1240 | hook itself (self-call only) | spends native or the ERC20 quote out of the hook … | stale |
| `CauldronHook.fundLegacyBuffer` | CauldronHook.sol:1288 | anyone | receives native; it is credited to `legacyBuffer`… | fresh |
| `CauldronHook.sweepLegacyReserve` | CauldronHook.sol:1336 | legacyRegistry | transfers `token` to the caller-supplied recipien… | fresh |
| `CauldronHook.setSnipeParams` | CauldronHook.sol:1710 | owner | NONE | fresh |
| `CauldronHook.linkVolume` | CauldronHook.sol:1800 | registry | NONE | fresh |
| `CauldronHook.setDefaultTaxBps` | CauldronHook.sol:1912 | owner | NONE | fresh |
| `CauldronHook.releaseRelaunchETH` | CauldronHook.sol:1927 | registry | sends native to `registry` (line 1859) | fresh |
| `CauldronHook.releaseRelaunchAsset` | CauldronHook.sol:1955 | registry | ERC20 transfer of `asset` to `registry` (line 188… | fresh |
| `CauldronHook.setDeathThreshold` | CauldronHook.sol:2037 | owner | NONE | fresh |
| `CauldronHook.forceClosePerps` | CauldronHook.sol:2060 | registry | NONE | fresh |
| `CauldronHook.setLiveKey` | CauldronHook.sol:2122 | registry | NONE | stale |
| `CauldronHook.setLegacyBuyback` | CauldronHook.sol:2136 | owner or registry | NONE | stale |
| `CauldronHook.setDeathChecker` | CauldronHook.sol:2198 | owner or registry | NONE | fresh |
| `CauldronHook.setPolicies` | CauldronHook.sol:2208 | owner or registry | NONE | fresh |
| `CauldronHook.setFeeRouter` | CauldronHook.sol:2220 | owner or registry | NONE | fresh |
| `CauldronHook.setNftContract` | CauldronHook.sol:2226 | owner | NONE | fresh |
| `CauldronHook.setRegistry` | CauldronHook.sol:2239 | owner | NONE | fresh |
| `CauldronHook.proposeRegistryOverride` | CauldronHook.sol:2255 | owner | NONE | fresh |
| `CauldronHook.cancelRegistryOverride` | CauldronHook.sol:2264 | owner | NONE | fresh |
| `CauldronHook.executeRegistryOverride` | CauldronHook.sol:2275 | owner | sends native to the OUTGOING `registry` (line 215… | fresh |
| `CauldronHook.setCollection` | CauldronHook.sol:2307 | registry | NONE | fresh |
| `CauldronHook.setVault` | CauldronHook.sol:2335 | registry | NONE | fresh |
| `CauldronHook.setQuest` | CauldronHook.sol:2341 | owner or registry | NONE | fresh |
| `CauldronHook.setSeeder` | CauldronHook.sol:2350 | owner or registry | NONE | fresh |
| `CauldronHook.setPerpEngine` | CauldronHook.sol:2358 | owner or registry | NONE | fresh |
| `CauldronHook.setFloorBps` | CauldronHook.sol:2367 | owner | NONE | fresh |
| `CauldronHook.setGuild` | CauldronHook.sol:2374 | owner or registry | NONE | fresh |
| `CauldronHook.setGuildBps` | CauldronHook.sol:2386 | owner | NONE | fresh |
| `CauldronHook.setActiveProposer` | CauldronHook.sol:2394 | owner or registry | NONE | fresh |
| `CauldronHook.setProposerBps` | CauldronHook.sol:2401 | owner | NONE | fresh |
| `CauldronHook.claimProposerFees` | CauldronHook.sol:2409 | anyone (each caller can only claim their own accrued balance) | sends native to the caller via `call` (line 2288) | fresh |
| `CauldronHook.setNftCurve` | CauldronHook.sol:2419 | owner | NONE | fresh |
| `CauldronHook.setCreditUntaggedSwaps` | CauldronHook.sol:2425 | owner | NONE | fresh |
| `CauldronHook.setNftCurveFrom` | CauldronHook.sol:2432 | registry | NONE | fresh |
| `CauldronHook.commitCrystals` | CauldronHook.sol:2545 | opener (an address flagged in isOpener) | NONE | fresh |
| `CauldronHook.resolveTickets` | CauldronHook.sol:2625 | anyone | NONE | fresh |
| `CauldronHook.nativeGachaStep` | CauldronHook.sol:2655 | hook itself (self-call only) | NONE | fresh |
| `CauldronHook.setOpener` | CauldronHook.sol:2664 | owner or registry | NONE | fresh |
| `CauldronHook.setTaxExempt` | CauldronHook.sol:2674 | owner or registry | NONE | fresh |
| `CauldronHook.setOddsParams` | CauldronHook.sol:2704 | owner | NONE | fresh |
| `CauldronHook.setMaxOdds` | CauldronHook.sol:2711 | owner | NONE | fresh |
| `CauldronHook.setWeights` | CauldronHook.sol:2717 | owner | NONE | fresh |
| `FeeRouteLib.routeSplit` | cauldron/FeeRouteLib.sol:48 | internal to the hook via delegatecall (callers: CauldronHook._routeEt… | no direct transfer; value leaves through `_fundGu… | fresh |
| `FeeRouteLib.routePerp` | cauldron/FeeRouteLib.sol:79 | internal to the hook via delegatecall (caller: CauldronHook._routePer… | no direct transfer; value leaves through `_fundGu… | fresh |
| `FeeRouteLib.send` | cauldron/FeeRouteLib.sol:201 | internal to the hook via delegatecall (caller: CauldronHook.releaseRe… | sends native to `to` (line 207); sends native wit… | fresh |
| `FeeRouteLib.deliver` | cauldron/FeeRouteLib.sol:231 | internal to the hook via delegatecall; NO caller anywhere in the sour… | sends native to `to` (line 251); ERC20 approve of… | stale |
| `LegacyBuyLib.buyStep` | cauldron/LegacyBuyLib.sol:132 | anyone at the linked library address; in protocol delegatecalled only… | sends native to `poolManager` (LegacyBuyLib.sol:2… | fresh |
| `RoyaltyRouter.sweep` | cauldron/RoyaltyRouter.sol:101 | anyone | sends held native to `hook` (line 108), or all he… | fresh |

## registry

Files: `CauldronRegistry.sol`, `CauldronToken.sol`, `cauldron/IPolicies.sol`, `cauldron/IDeathChecker.sol`, `cauldron/ILiquidatorMintable.sol`, `cauldron/ICauldron.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `CauldronRegistry.setRedemptionExt` | CauldronRegistry.sol:190 | owner | NONE | fresh |
| `CauldronRegistry.setReserveCeiling` | CauldronRegistry.sol:203 | owner | NONE | fresh |
| `CauldronRegistry.rotateSlice` | CauldronRegistry.sol:246 | anyone at the registry; the facet holds the gate (RedemptionExt.rotat… | NONE in this stub; the value movement happens ins… | fresh |
| `CauldronRegistry.rotateSliceFrom` | CauldronRegistry.sol:270 | anyone at the registry; the facet holds the gate (RedemptionExt.rotat… | NONE in this stub; the value movement happens ins… | fresh |
| `CauldronRegistry.setRotationWiring` | CauldronRegistry.sol:280 | owner (the gate is on the facet: RedemptionExt.setRotationWiring is o… | NONE | fresh |
| `CauldronRegistry.recoverLegs` | CauldronRegistry.sol:296 | anyone at the registry; the facet holds the only gate (RedemptionExt.… | NONE in this stub; the facet moves the recovered … | fresh |
| `CauldronRegistry.sweepLegProceeds` | CauldronRegistry.sol:306 | owner (the gate is on the facet: RedemptionExt.sweepLegProceeds is on… | NONE in this stub; the facet sends the booked ass… | fresh |
| `CauldronRegistry.setAllowedQuote` | CauldronRegistry.sol:314 | owner | NONE | fresh |
| `CauldronRegistry.setSeeder` | CauldronRegistry.sol:333 | owner | NONE | fresh |
| `CauldronRegistry.rescueSeeder` | CauldronRegistry.sol:345 | emergencyAdmin | receives native and/or token back from the seeder… | fresh |
| `CauldronRegistry.setSeedWindow` | CauldronRegistry.sol:356 | owner | NONE | fresh |
| `CauldronRegistry.setIgniter` | CauldronRegistry.sol:420 | owner | NONE | fresh |
| `CauldronRegistry.armEmergency` | CauldronRegistry.sol:426 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.setGuardian` | CauldronRegistry.sol:434 | emergencyAdmin or owner | NONE | fresh |
| `CauldronRegistry.vetoEmergency` | CauldronRegistry.sol:444 | guardian | NONE | fresh |
| `CauldronRegistry.setRedemptionPaused` | CauldronRegistry.sol:459 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.setEnchantFeeMult` | CauldronRegistry.sol:468 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.emergencyWithdrawLP` | CauldronRegistry.sol:475 | emergencyAdmin | ERC20 transfer of the generation token `tok` to t… | fresh |
| `CauldronRegistry.emergencySweep` | CauldronRegistry.sol:488 | emergencyAdmin | sends the registry's whole native balance to `eme… | fresh |
| `CauldronRegistry.setSuccessor` | CauldronRegistry.sol:505 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.setClaimGate` | CauldronRegistry.sol:524 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.migrateToSuccessor` | CauldronRegistry.sol:540 | emergencyAdmin | ERC721 ownership of the active position is moved … | fresh |
| `CauldronRegistry.setGovernor` | CauldronRegistry.sol:585 | owner | NONE | fresh |
| `CauldronRegistry.setMinLifetime` | CauldronRegistry.sol:590 | emergencyAdmin | NONE | fresh |
| `CauldronRegistry.setFactory` | CauldronRegistry.sol:595 | owner | NONE | fresh |
| `CauldronRegistry.setNftMaxSupply` | CauldronRegistry.sol:600 | owner | NONE | fresh |
| `CauldronRegistry.setRoyalty` | CauldronRegistry.sol:607 | owner | NONE | fresh |
| `CauldronRegistry.setGenesisMetadata` | CauldronRegistry.sol:615 | owner | NONE | fresh |
| `CauldronRegistry.setCollectionMetadata` | CauldronRegistry.sol:643 | caller satisfying `onlyOwner` | NONE | fresh |
| `CauldronRegistry.setGenesisBonus` | CauldronRegistry.sol:661 | owner | NONE | fresh |
| `CauldronRegistry.setAirdropReserve` | CauldronRegistry.sol:676 | owner | NONE | fresh |
| `CauldronRegistry.setPrimeFunder` | CauldronRegistry.sol:685 | owner | NONE | fresh |
| `CauldronRegistry.fundPrimeBuy` | CauldronRegistry.sol:695 | primeFunder | receives native, accumulated into `primeBuyEth` (… | fresh |
| `CauldronRegistry.sweepPrimeBuy` | CauldronRegistry.sol:701 | primeFunder | sends native equal to `amt` back to the funder (l… | fresh |
| `CauldronRegistry.summon` | CauldronRegistry.sol:722 | owner or igniter | receives native as the entire genesis pairing, wh… | fresh |
| `CauldronRegistry.relaunch` | CauldronRegistry.sol:821 | anyone | NONE | fresh |
| `CauldronRegistry.claimByBurn` | CauldronRegistry.sol:1291 | anyone (holders of a previous generation's token); blocked for ordina… | burns the caller's previous-generation balance an… | fresh |
| `CauldronRegistry.claimByBurnUpTo` | CauldronRegistry.sol:1336 | any holder of an earlier generation, unless a claim gate is set, in w… | NONE in this stub; the burn and the reserve withd… | fresh |
| `CauldronRegistry.enableAutoMigrate` | CauldronRegistry.sol:1354 | anyone (free for any MiFren holder, otherwise a fee is required) | receives native: the opt-in fee is required only … | fresh |
| `CauldronRegistry.disableAutoMigrate` | CauldronRegistry.sol:1382 | anyone (for their own wallet only) | NONE | fresh |
| `CauldronRegistry.autoMigrateBatch` | CauldronRegistry.sol:1394 | anyone (permissionless keeper) | burns each opted-in holder's whole previous-gener… | fresh |
| `CauldronRegistry.redeemOgFren` | CauldronRegistry.sol:1437 | anyone holding a genesis fren; the facet holds the gate (RedemptionEx… | NONE in this stub; the facet pays the live floor … | fresh |
| `CauldronRegistry.buyTreasuryOgFren` | CauldronRegistry.sol:1443 | anyone; the facet holds the gate (the fren must currently sit in this… | NONE in this stub; the facet pulls twice the live… | fresh |
| `CauldronRegistry.donateToReserve` | CauldronRegistry.sol:1449 | anyone | NONE in this stub; the facet pulls the donated am… | fresh |
| `CauldronRegistry.materializeLegacyReserve` | CauldronRegistry.sol:1455 | anyone (permissionless keeper) | NONE in this stub; the facet sweeps the hook's he… | fresh |
| `CauldronRegistry.floorClaimableNow` | CauldronRegistry.sol:1472 | anyone | NONE | fresh |
| `CauldronRegistry.legCount` | CauldronRegistry.sol:1477 | anyone | NONE | fresh |
| `CauldronRegistry.legAt` | CauldronRegistry.sol:1482 | anyone | NONE | fresh |
| `CauldronRegistry.setCollectionLedger` | CauldronRegistry.sol:1527 | owner | NONE | fresh |
| `CauldronRegistry.recycleCollectionNFT` | CauldronRegistry.sol:1562 | anyone owning the collection NFT (ownership enforced in the library) | releases the collection's floor entitlement in th… | fresh |
| `CauldronRegistry.buyCollectionNFT` | CauldronRegistry.sol:1583 | anyone (the NFT must currently sit in this registry's treasury) | pulls twice the NFT's floor in the live token fro… | fresh |
| `CauldronRegistry.unlockCallback` | CauldronRegistry.sol:1812 | poolManager, and only while the seed-buy window is armed | spends this registry's quote side and receives th… | fresh |
| `CauldronToken.burn` | CauldronToken.sol:58 | registry | destroys `amount` of the named holder's balance, … | fresh |

## pool

Files: `cauldron/CauldronBase.sol`, `cauldron/PoolOps.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `PoolOps.createAndSeed` | cauldron/PoolOps.sol:262 | anyone directly at the linked library address; NO in-protocol caller | NONE directly; the quote leg is paid inside `_see… | fresh |
| `PoolOps.createAndSeedProgressive` | cauldron/PoolOps.sol:317 | anyone | receives native through `msg.value` (PoolOps.sol:… | fresh |
| `PoolOps.createAndSeedWithBuy` | cauldron/PoolOps.sol:446 | anyone directly at the linked library address; in the protocol path, … | NONE directly; the whole quote tranche is spent i… | fresh |
| `PoolOps.executeBuy` | cauldron/PoolOps.sol:569 | the PoolManager, through the registry's armed unlock window (the gate… | sends native to the PoolManager at `settle` (line… | fresh |
| `PoolOps.primeBuy` | cauldron/PoolOps.sol:652 | anyone directly at the linked library address; in the protocol path, … | spends exactly `ethIn` of the registry's native b… | fresh |
| `PoolOps.deployTokenAbove` | cauldron/PoolOps.sol:753 | anyone directly at the linked library address; in the protocol path, … | NONE (the deployed token mints its whole fixed su… | fresh |
| `PoolOps.openOrAddPair` | cauldron/PoolOps.sol:905 | anyone directly at the linked library address; in the protocol path, … | the quote and token legs are paid inside `_seedAc… | fresh |
| `PoolOps.removePartial` | cauldron/PoolOps.sol:976 | anyone directly at the linked library address; in the protocol path, … | TAKE_PAIR delivers both currencies to the registr… | fresh |
| `PoolOps.seedFunding` | cauldron/PoolOps.sol:1071 | anyone in the linked library; protocol execution is reached only thro… | NONE (best-effort calls may move vault native and… | fresh |
| `PoolOps.sendAsset` | cauldron/PoolOps.sol:1249 | anyone directly at the linked library address; in the protocol path, … | sends native to an arbitrary recipient at `call` … | fresh |
| `PoolOps.removeAll` | cauldron/PoolOps.sol:1280 | anyone directly at the linked library address; in the protocol path, … | TAKE_PAIR delivers both currencies to the registr… | fresh |
| `PoolOps.claimFromReserve` | cauldron/PoolOps.sol:1311 | anyone directly at the linked library address; in the protocol path, … | delivers exactly the claimed token amount to `rec… | fresh |
| `PoolOps.addToReserve` | cauldron/PoolOps.sol:1351 | anyone directly at the linked library address; in the protocol path, … | the token is PULLED from the registry into the po… | fresh |
| `PoolOps.migrateUpTo` | cauldron/PoolOps.sol:1401 | anyone directly at the linked library address; in the protocol path, … | burns the caller's old token and delivers the sam… | fresh |
| `PoolOps.migrateOne` | cauldron/PoolOps.sol:1412 | anyone directly at the linked library address; in the protocol path, … | burns `amount` of the previous generation's token… | fresh |
| `PoolOps.autoMigrateBatch` | cauldron/PoolOps.sol:1438 | anyone directly at the linked library address; in the protocol path, … | per holder, burns their whole previous-generation… | fresh |
| `PoolOps.doLegacyNote` | cauldron/PoolOps.sol:1462 | anyone directly at the linked library address; in the protocol path, … | NONE | fresh |
| `PoolOps.materializeLegacy` | cauldron/PoolOps.sol:1493 | anyone directly at the linked library address; in the protocol path, … | ERC20 transfer of the hook's held live-buyback to… | fresh |
| `PoolOps.crystallizeCollection` | cauldron/PoolOps.sol:1518 | anyone directly at the linked library address; in the protocol path, … | NONE | fresh |
| `PoolOps.recycleCollection` | cauldron/PoolOps.sol:1576 | anyone at the linked library; protocol path is the registry's externa… | moves the NFT into registry custody and releases … | fresh |
| `PoolOps.buyCollection` | cauldron/PoolOps.sol:1625 | anyone at the linked library; protocol path is the registry's externa… | pulls payment tokens from `caller`, adds them to … | stale |

## perp

Files: `cauldron/PerpEngine.sol`, `cauldron/PerpVault.sol`, `cauldron/PerpSwapLib.sol`, `cauldron/PerpMarkSource.sol`, `cauldron/PerpStakerOracle.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `PerpEngine.poke` | cauldron/PerpEngine.sol:758 | anyone | NONE | fresh |
| `PerpEngine.openLong` | cauldron/PerpEngine.sol:1003 | anyone | NONE | fresh |
| `PerpEngine.openShort` | cauldron/PerpEngine.sol:1034 | anyone | NONE | fresh |
| `PerpEngine.close` | cauldron/PerpEngine.sol:1063 | caller restricted by explicit msg.sender check | NONE | fresh |
| `PerpEngine.liquidate` | cauldron/PerpEngine.sol:1070 | anyone | NONE | fresh |
| `PerpEngine.sweepLiquidations` | cauldron/PerpEngine.sol:1127 | ? | ? | missing |
| `PerpEngine.selfSweep` | cauldron/PerpEngine.sol:1151 | caller restricted by explicit msg.sender check | NONE | fresh |
| `PerpEngine.forceCloseDead` | cauldron/PerpEngine.sol:1482 | anyone | NONE | fresh |
| `PerpEngine.forceCloseAllDead` | cauldron/PerpEngine.sol:1495 | anyone | NONE | fresh |
| `PerpEngine.requoteBook` | cauldron/PerpEngine.sol:1550 | ? | ? | missing |
| `PerpEngine.syncGeneration` | cauldron/PerpEngine.sol:1571 | caller restricted by explicit msg.sender check | NONE | stale |
| `PerpEngine.unlockCallback` | cauldron/PerpEngine.sol:2134 | caller restricted by explicit msg.sender check | NONE | fresh |
| `PerpEngine.retirePayout` | cauldron/PerpEngine.sol:2493 | anyone | NONE | fresh |
| `PerpEngine.claimPayout` | cauldron/PerpEngine.sol:2528 | anyone | NONE | fresh |
| `PerpEngine.claimLiquidatorBadges` | cauldron/PerpEngine.sol:2714 | anyone | NONE | fresh |
| `PerpEngine.fundPlv` | cauldron/PerpEngine.sol:2740 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.fundPlvToken` | cauldron/PerpEngine.sol:2748 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.fundInsurance` | cauldron/PerpEngine.sol:2759 | anyone | NONE | fresh |
| `PerpEngine.creditPerpFee` | cauldron/PerpEngine.sol:2769 | anyone | NONE | fresh |
| `PerpEngine.creditPerpFeeToken` | cauldron/PerpEngine.sol:2774 | anyone | NONE | fresh |
| `PerpEngine.creditPerpFeeAsset` | cauldron/PerpEngine.sol:2795 | caller restricted by explicit msg.sender check | NONE | fresh |
| `PerpEngine.fundFromVault` | cauldron/PerpEngine.sol:2844 | configured vault via onlyVault | NONE | fresh |
| `PerpEngine.withdrawPlvTo` | cauldron/PerpEngine.sol:2850 | configured vault via onlyVault | NONE | fresh |
| `PerpEngine.withdrawTokYieldTo` | cauldron/PerpEngine.sol:2862 | configured vault via onlyVault | NONE | fresh |
| `PerpEngine.fundTokenFromVault` | cauldron/PerpEngine.sol:2867 | configured vault via onlyVault | NONE | fresh |
| `PerpEngine.withdrawPlvTokenTo` | cauldron/PerpEngine.sol:2872 | configured vault via onlyVault | NONE | fresh |
| `PerpEngine.setFees` | cauldron/PerpEngine.sol:2879 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setRisk` | cauldron/PerpEngine.sol:2883 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setTiers` | cauldron/PerpEngine.sol:2920 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setRouting` | cauldron/PerpEngine.sol:2942 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setVaultSplit` | cauldron/PerpEngine.sol:2961 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setGuards` | cauldron/PerpEngine.sol:2966 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setVault` | cauldron/PerpEngine.sol:3000 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setVaultLimits` | cauldron/PerpEngine.sol:3006 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.setMinCollateral` | cauldron/PerpEngine.sol:3011 | owner via onlyOwner | NONE | fresh |
| `PerpEngine.skimInsurance` | cauldron/PerpEngine.sol:3027 | owner via onlyOwner | NONE | fresh |
| `PerpMarkSource.setPrimary` | cauldron/PerpMarkSource.sol:115 | owner via onlyOwner | NONE | fresh |
| `PerpMarkSource.addPool` | cauldron/PerpMarkSource.sol:126 | owner via onlyOwner | NONE | fresh |
| `PerpMarkSource.removePool` | cauldron/PerpMarkSource.sol:147 | owner via onlyOwner | NONE | fresh |
| `PerpSwapLib.writeObs` | cauldron/PerpSwapLib.sol:390 | anyone | NONE | fresh |
| `PerpSwapLib.tryMintBadge` | cauldron/PerpSwapLib.sol:437 | anyone | NONE | fresh |
| `PerpSwapLib.tryTransferFrom` | cauldron/PerpSwapLib.sol:490 | anyone | NONE | fresh |
| `PerpSwapLib.tryTransfer` | cauldron/PerpSwapLib.sol:498 | anyone | NONE | fresh |
| `PerpSwapLib.swapLeg` | cauldron/PerpSwapLib.sol:532 | anyone | NONE | fresh |
| `PerpSwapLib.migrateInventory` | cauldron/PerpSwapLib.sol:627 | anyone | NONE | fresh |
| `PerpSwapLib.syncQuoteChangeAt` | cauldron/PerpSwapLib.sol:968 | ? | ? | missing |
| `PerpSwapLib.requoteBookAt` | cauldron/PerpSwapLib.sol:1032 | ? | ? | missing |
| `PerpVault.deposit` | cauldron/PerpVault.sol:319 | anyone | receives or validates `msg.value` (line 316) | stale |
| `PerpVault.depositEth` | cauldron/PerpVault.sol:362 | anyone | receives or validates `msg.value` (line 334) | fresh |
| `PerpVault.withdrawEth` | cauldron/PerpVault.sol:390 | anyone | NONE | stale |
| `PerpVault.beforeBookRequote` | cauldron/PerpVault.sol:460 | ? | ? | missing |
| `PerpVault.afterBookRequote` | cauldron/PerpVault.sol:482 | ? | ? | missing |
| `PerpVault.settlePendingEth` | cauldron/PerpVault.sol:616 | anyone | NONE | fresh |
| `PerpVault.claimPendingEth` | cauldron/PerpVault.sol:628 | caller restricted by explicit msg.sender check | NONE | stale |
| `PerpVault.depositToken` | cauldron/PerpVault.sol:748 | anyone | NONE | fresh |
| `PerpVault.claimTokYield` | cauldron/PerpVault.sol:776 | anyone | NONE | stale |
| `PerpVault.withdrawToken` | cauldron/PerpVault.sol:788 | anyone | NONE | fresh |
| `PerpVault.settlePendingToken` | cauldron/PerpVault.sol:855 | anyone | NONE | fresh |
| `PerpVault.claimPendingToken` | cauldron/PerpVault.sol:870 | caller restricted by explicit msg.sender check | NONE | fresh |

## rotation

Files: `cauldron/QuoteRotator.sol`, `cauldron/QuoteOracle.sol`, `cauldron/RedemptionExt.sol`, `cauldron/CauldronVault.sol`, `cauldron/NativeQuoteZap.sol`, `cauldron/MockAggregator.sol`, `cauldron/MockQuoteToken.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `CauldronVault.redeem` | cauldron/CauldronVault.sol:161 | caller restricted by explicit msg.sender check | sends native through `value` (line 170) | stale |
| `CauldronVault.close` | cauldron/CauldronVault.sol:197 | caller restricted by explicit msg.sender check | sends native through `value` (line 191) | fresh |
| `MockAggregator.transferOwnership` | cauldron/MockAggregator.sol:65 | owner via onlyOwner | NONE | fresh |
| `MockAggregator.peg` | cauldron/MockAggregator.sol:68 | owner via onlyOwner | NONE | fresh |
| `MockAggregator.setStale` | cauldron/MockAggregator.sol:76 | owner via onlyOwner | NONE | fresh |
| `MockAggregator.setDown` | cauldron/MockAggregator.sol:82 | owner via onlyOwner | NONE | fresh |
| `MockQuoteToken.mint` | cauldron/MockQuoteToken.sol:27 | anyone | NONE | fresh |
| `NativeQuoteZap.zap` | cauldron/NativeQuoteZap.sol:101 | anyone | receives or validates `msg.value` (line 102) | fresh |
| `NativeQuoteZap.unlockCallback` | cauldron/NativeQuoteZap.sol:123 | caller restricted by explicit msg.sender check | sends native through `value` (line 147) | fresh |
| `QuoteOracle.transferOwnership` | cauldron/QuoteOracle.sol:119 | owner via onlyOwner | NONE | fresh |
| `QuoteOracle.setFeed` | cauldron/QuoteOracle.sol:131 | owner via onlyOwner | NONE | fresh |
| `QuoteOracle.setPegged` | cauldron/QuoteOracle.sol:153 | owner via onlyOwner | NONE | fresh |
| `QuoteOracle.setBounds` | cauldron/QuoteOracle.sol:175 | owner via onlyOwner | NONE | fresh |
| `QuoteOracle.setSequencer` | cauldron/QuoteOracle.sol:182 | owner via onlyOwner | NONE | fresh |
| `QuoteOracle.cachedUsdPerRawUnit` | cauldron/QuoteOracle.sol:334 | anyone | NONE | fresh |
| `QuoteRotator.transferOwnership` | cauldron/QuoteRotator.sol:155 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.setVenue` | cauldron/QuoteRotator.sol:210 | owner via onlyOwner | NONE | stale |
| `QuoteRotator.setPlan` | cauldron/QuoteRotator.sol:263 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.cancelPlan` | cauldron/QuoteRotator.sol:295 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.setKeeperBps` | cauldron/QuoteRotator.sol:300 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.rotateStep` | cauldron/QuoteRotator.sol:331 | anyone | NONE | fresh |
| `QuoteRotator.swapOnce` | cauldron/QuoteRotator.sol:382 | anyone | NONE | stale |
| `QuoteRotator.setRotationSlipBps` | cauldron/QuoteRotator.sol:451 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.setArbParams` | cauldron/QuoteRotator.sol:529 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.setMaxArbNotionalUsd` | cauldron/QuoteRotator.sol:539 | owner via onlyOwner | NONE | fresh |
| `QuoteRotator.arbStep` | cauldron/QuoteRotator.sol:577 | anyone | NONE | fresh |
| `QuoteRotator.withdraw` | cauldron/QuoteRotator.sol:697 | caller restricted by explicit msg.sender check | NONE | stale |
| `QuoteRotator.unlockCallback` | cauldron/QuoteRotator.sol:724 | caller restricted by explicit msg.sender check | NONE | fresh |
| `RedemptionExt.redeemOgFren` | cauldron/RedemptionExt.sol:80 | caller restricted by explicit msg.sender check | NONE | stale |
| `RedemptionExt.buyTreasuryOgFren` | cauldron/RedemptionExt.sol:121 | anyone | NONE | stale |
| `RedemptionExt.donateToReserve` | cauldron/RedemptionExt.sol:147 | anyone | NONE | fresh |
| `RedemptionExt.materializeLegacyReserve` | cauldron/RedemptionExt.sol:158 | anyone | NONE | stale |
| `RedemptionExt.setRotationWiring` | cauldron/RedemptionExt.sol:286 | owner via onlyOwner | NONE | fresh |
| `RedemptionExt.rotateSlice` | cauldron/RedemptionExt.sol:295 | anyone | NONE | fresh |
| `RedemptionExt.rotateSliceFrom` | cauldron/RedemptionExt.sol:306 | anyone | NONE | stale |
| `RedemptionExt.claimByBurnUpTo` | cauldron/RedemptionExt.sol:794 | caller restricted by explicit msg.sender check | NONE | fresh |
| `RedemptionExt.recoverLegs` | cauldron/RedemptionExt.sol:914 | anyone | NONE | fresh |
| `RedemptionExt.recoverLegsAtTeardown` | cauldron/RedemptionExt.sol:965 | anyone | NONE | fresh |
| `RedemptionExt.sweepLegProceeds` | cauldron/RedemptionExt.sol:1064 | owner via onlyOwner | NONE | fresh |

## nft

Files: `cauldron/MiFrensGenesis.sol`, `cauldron/CauldronCollection.sol`, `cauldron/CollectionLedger.sol`, `cauldron/CauldronGachaRouter.sol`, `cauldron/GachaLib.sol`, `cauldron/MiFrensDividend.sol`, `cauldron/MintCurvePolicy.sol`, `cauldron/CauldronFactory.sol`, `cauldron/ICreatorToken.sol`, `interfaces/INFTContract.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `CauldronCollection.setTransferValidator` | cauldron/CauldronCollection.sol:192 | registry (held in the immutable `deployer` slot) | NONE | fresh |
| `CauldronCollection.mint` | cauldron/CauldronCollection.sol:207 | minter (the volume hook, frozen at deploy) | NONE | fresh |
| `CauldronCollection.reveal` | cauldron/CauldronCollection.sol:219 | holder of token | NONE | fresh |
| `CauldronCollection.revealBatch` | cauldron/CauldronCollection.sol:239 | holder of token | NONE | fresh |
| `CauldronCollection.setVault` | cauldron/CauldronCollection.sol:331 | configurator (the factory) or registry | NONE | fresh |
| `CauldronCollection.setRoyalty` | cauldron/CauldronCollection.sol:341 | configurator (the factory) or registry | NONE | fresh |
| `CauldronCollection.setLiquidatorMinter` | cauldron/CauldronCollection.sol:351 | registry or minter (the volume hook) | NONE | fresh |
| `CauldronCollection.setMetadata` | cauldron/CauldronCollection.sol:361 | registry (held in the immutable `deployer` slot) | NONE | fresh |
| `CauldronCollection.setLiquidatorURI` | cauldron/CauldronCollection.sol:373 | registry (held in the immutable `deployer` slot) | NONE | fresh |
| `CauldronCollection.setUnrevealedURI` | cauldron/CauldronCollection.sol:388 | deployer (registry controller) | NONE | fresh |
| `CauldronCollection.mintLiquidator` | cauldron/CauldronCollection.sol:396 | liquidatorMinter (the wired PerpEngine) | NONE | fresh |
| `CauldronCollection.mintLiquidatorWithStats` | cauldron/CauldronCollection.sol:402 | liquidatorMinter (the wired PerpEngine) | NONE | fresh |
| `CauldronCollection.setLiquidatorRenderer` | cauldron/CauldronCollection.sol:433 | configurator (the factory) or registry | NONE | fresh |
| `CauldronCollection.burnFromVault` | cauldron/CauldronCollection.sol:444 | vault | NONE | fresh |
| `CauldronCollection.custodyTransfer` | cauldron/CauldronCollection.sol:454 | registry (held in the immutable `deployer` slot) | NONE | fresh |
| `CauldronCollection.setRarityOdds` | cauldron/CauldronCollection.sol:486 | registry (held in the immutable `deployer` slot) | NONE | fresh |
| `CauldronFactory.setLiquidatorRenderer` | cauldron/CauldronFactory.sol:36 | owner | NONE | fresh |
| `CauldronFactory.transferOwnership` | cauldron/CauldronFactory.sol:42 | owner | NONE | fresh |
| `CauldronFactory.deployBrew` | cauldron/CauldronFactory.sol:63 | anyone | No funds move; the call deploys `CauldronCollecti… | fresh |
| `CauldronFactory.deployVault` | cauldron/CauldronFactory.sol:105 | anyone | no native or token value moves; the call is a DEP… | fresh |
| `CauldronGachaRouter.setOracle` | cauldron/CauldronGachaRouter.sol:99 | owner | NONE | fresh |
| `CauldronGachaRouter.play` | cauldron/CauldronGachaRouter.sol:235 | anyone | receives native when the generation's quote is na… | fresh |
| `CauldronGachaRouter.playLiq` | cauldron/CauldronGachaRouter.sol:250 | anyone | receives native when the generation's quote is na… | fresh |
| `CauldronGachaRouter.openReady` | cauldron/CauldronGachaRouter.sol:347 | anyone | NONE | fresh |
| `CauldronGachaRouter.playChurn` | cauldron/CauldronGachaRouter.sol:379 | anyone | receives native when native quote is selected; re… | fresh |
| `CauldronGachaRouter.unlockCallback` | cauldron/CauldronGachaRouter.sol:409 | poolManager (re-entered during unlock) | the pool manager pays the swap output to the play… | fresh |
| `CauldronGachaRouter.rescueETH` | cauldron/CauldronGachaRouter.sol:579 | owner | sends native to `to` (line 580) | fresh |
| `CauldronGachaRouter.rescueToken` | cauldron/CauldronGachaRouter.sol:592 | owner | sends `amount` of an arbitrary ERC20 to `to` (lin… | fresh |
| `CollectionLedger.credit` | cauldron/CollectionLedger.sol:146 | registry | NONE | fresh |
| `CollectionLedger.redeem` | cauldron/CollectionLedger.sol:161 | registry | NONE | fresh |
| `CollectionLedger.buyback` | cauldron/CollectionLedger.sol:174 | registry | NONE | fresh |
| `CollectionLedger.crystallize` | cauldron/CollectionLedger.sol:187 | registry | NONE | fresh |
| `GachaLib.resolveTickets` | cauldron/GachaLib.sol:173 | anyone at linked library address; protocol reaches it by delegatecall… | NONE | fresh |
| `MiFrensDividend.setRegistry` | cauldron/MiFrensDividend.sol:189 | treasury | NONE | fresh |
| `MiFrensDividend.setFunder` | cauldron/MiFrensDividend.sol:199 | treasury | NONE | fresh |
| `MiFrensDividend.fundToken` | cauldron/MiFrensDividend.sol:273 | funder (hook), wired once by treasury | pulls requested ERC20 `amount` of `asset` from fu… | fresh |
| `MiFrensDividend.adopt` | cauldron/MiFrensDividend.sol:323 | anyone for known assets; funder or treasury for a new asset | books ERC20 balance already held by this contract… | fresh |
| `MiFrensDividend.claimTokens` | cauldron/MiFrensDividend.sol:357 | holder of token who is also its caster | ERC20 push of `a` to `msg.sender` (line 375) | fresh |
| `MiFrensDividend.withdrawOwedToken` | cauldron/MiFrensDividend.sol:383 | anyone (pays only the caller's own banked balance) | ERC20 push of `asset` to `msg.sender` (line 390) | fresh |
| `MiFrensDividend.castSpell` | cauldron/MiFrensDividend.sol:434 | holder of token | NONE | fresh |
| `MiFrensDividend.castMany` | cauldron/MiFrensDividend.sol:439 | holder of token | NONE | fresh |
| `MiFrensDividend.onMiFrenTransfer` | cauldron/MiFrensDividend.sol:523 | the MiFrens collection | NONE | fresh |
| `MiFrensDividend.claim` | cauldron/MiFrensDividend.sol:561 | holder of token who is also its caster | sends native to `msg.sender` (line 597) | fresh |
| `MiFrensDividend.claimMany` | cauldron/MiFrensDividend.sol:566 | holder of token who is also its caster | sends native to `msg.sender` (line 597) | fresh |
| `MiFrensDividend.withdrawOwed` | cauldron/MiFrensDividend.sol:577 | anyone (pays only the caller's own banked balance) | sends native to `msg.sender` (line 582) | fresh |
| `MiFrensGenesis.setMaxPerWallet` | cauldron/MiFrensGenesis.sol:356 | deployer | NONE | fresh |
| `MiFrensGenesis.setUnrevealedURI` | cauldron/MiFrensGenesis.sol:372 | deployer | NONE | fresh |
| `MiFrensGenesis.setRegistry` | cauldron/MiFrensGenesis.sol:378 | deployer | NONE | fresh |
| `MiFrensGenesis.mint` | cauldron/MiFrensGenesis.sol:391 | anyone | receives native - exact `msg.value` required (lin… | stale |
| `MiFrensGenesis.mintDiscounted` | cauldron/MiFrensGenesis.sol:398 | ? | ? | missing |
| `MiFrensGenesis.setDiscountRoot` | cauldron/MiFrensGenesis.sol:414 | ? | ? | missing |
| `MiFrensGenesis.setDiscountSetter` | cauldron/MiFrensGenesis.sol:421 | ? | ? | missing |
| `MiFrensGenesis.cancelPresale` | cauldron/MiFrensGenesis.sol:468 | deployer | NONE | fresh |
| `MiFrensGenesis.refund` | cauldron/MiFrensGenesis.sol:479 | anyone (pays out only to a caller with a recorded balance) | sends native to `msg.sender` (line 375) | fresh |
| `MiFrensGenesis.setMinter` | cauldron/MiFrensGenesis.sol:512 | deployer or registry | NONE | fresh |
| `MiFrensGenesis.setVault` | cauldron/MiFrensGenesis.sol:517 | deployer or registry | NONE | fresh |
| `MiFrensGenesis.setDividend` | cauldron/MiFrensGenesis.sol:522 | deployer or registry | NONE | fresh |
| `MiFrensGenesis.setLiquidatorMinter` | cauldron/MiFrensGenesis.sol:530 | deployer, registry, or the wired minter (the volume hook) | NONE | fresh |
| `MiFrensGenesis.setLiquidatorURI` | cauldron/MiFrensGenesis.sol:536 | deployer | NONE | fresh |
| `MiFrensGenesis.mintLiquidator` | cauldron/MiFrensGenesis.sol:544 | liquidatorMinter (the wired PerpEngine) | NONE | fresh |
| `MiFrensGenesis.mintLiquidatorWithStats` | cauldron/MiFrensGenesis.sol:549 | liquidatorMinter (the wired PerpEngine) | NONE | fresh |
| `MiFrensGenesis.setLiquidatorRenderer` | cauldron/MiFrensGenesis.sol:572 | deployer | NONE | fresh |
| `MiFrensGenesis.custodyTransfer` | cauldron/MiFrensGenesis.sol:593 | registry | NONE | fresh |
| `MiFrensGenesis.setFinalizer` | cauldron/MiFrensGenesis.sol:602 | deployer | NONE | fresh |
| `MiFrensGenesis.setMetadata` | cauldron/MiFrensGenesis.sol:608 | deployer | NONE | fresh |
| `MiFrensGenesis.setRarityOdds` | cauldron/MiFrensGenesis.sol:618 | deployer | NONE | fresh |
| `MiFrensGenesis.setRoyalty` | cauldron/MiFrensGenesis.sol:625 | deployer | NONE | fresh |
| `MiFrensGenesis.setTransferValidator` | cauldron/MiFrensGenesis.sol:644 | deployer | NONE | fresh |
| `MiFrensGenesis.mint` | cauldron/MiFrensGenesis.sol:653 | minter (the wired volume hook) | NONE | fresh |
| `MiFrensGenesis.reveal` | cauldron/MiFrensGenesis.sol:666 | holder of token | NONE | fresh |
| `MiFrensGenesis.revealBatch` | cauldron/MiFrensGenesis.sol:686 | holder of token | NONE | fresh |
| `MiFrensGenesis.burnFromVault` | cauldron/MiFrensGenesis.sol:758 | vault | NONE | fresh |
| `MiFrensGenesis.igniteCauldron` | cauldron/MiFrensGenesis.sol:788 | anyone once sold out - unless a finalizer is set, and then only the f… | forwards this contract's ENTIRE native balance to… | fresh |

## governance

Files: `cauldron/CauldronGovernor.sol`, `cauldron/TreasuryGovernor.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `CauldronGovernor.setRegistry` | cauldron/CauldronGovernor.sol:375 | owner | NONE | fresh |
| `CauldronGovernor.propose` | cauldron/CauldronGovernor.sol:443 | anyone | NONE | fresh |
| `CauldronGovernor.propose` | cauldron/CauldronGovernor.sol:472 | anyone | NONE | fresh |
| `CauldronGovernor.vote` | cauldron/CauldronGovernor.sol:644 | anyone holding checkpointed MiFrens voting power at the proposal's sn… | none - it only records votes; no asset moves (DER… | fresh |
| `CauldronGovernor.markConsumed` | cauldron/CauldronGovernor.sol:763 | registry | NONE | fresh |
| `TreasuryGovernor.propose` | cauldron/TreasuryGovernor.sol:415 | caller satisfying the in-body msg.sender check | NONE | fresh |
| `TreasuryGovernor.vote` | cauldron/TreasuryGovernor.sol:468 | anyone holding checkpointed MiFrens power at the proposal's snapshot | none - it only records votes (DERIVED) | fresh |
| `TreasuryGovernor.execute` | cauldron/TreasuryGovernor.sol:588 | anyone | NONE | fresh |
| `TreasuryGovernor.cancel` | cauldron/TreasuryGovernor.sol:651 | guardian | NONE | fresh |
| `TreasuryGovernor.setGuardian` | cauldron/TreasuryGovernor.sol:666 | guardian only | none (DERIVED) | fresh |
| `TreasuryGovernor.consume` | cauldron/TreasuryGovernor.sol:935 | caller satisfying the in-body msg.sender check | NONE | fresh |
| `TreasuryGovernor.setQuoteOracle` | cauldron/TreasuryGovernor.sol:1052 | guardian | NONE | fresh |

## seed

Files: `cauldron/CauldronSeeder.sol`, `cauldron/SeedLib.sol`, `cauldron/ISeeder.sol`, `cauldron/MigrationVesting.sol`, `cauldron/LaunchSniper.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `CauldronSeeder.startSeed` | cauldron/CauldronSeeder.sol:239 | registry | receives native through `msg.value` (line 243) an… | fresh |
| `CauldronSeeder.poke` | cauldron/CauldronSeeder.sol:303 | anyone | NONE | fresh |
| `CauldronSeeder.fundPrime` | cauldron/CauldronSeeder.sol:347 | deployer or registry owner | receives native through `msg.value` (line 363) | fresh |
| `CauldronSeeder.refundPrime` | cauldron/CauldronSeeder.sol:379 | deployer or registry owner | sends native to `to` through `call` (line 387) | fresh |
| `CauldronSeeder.pokeInSwap` | cauldron/CauldronSeeder.sol:465 | configured pool hook | NONE | fresh |
| `CauldronSeeder.unlockCallback` | cauldron/CauldronSeeder.sol:540 | poolManager | NONE | fresh |
| `CauldronSeeder.withdrawAll` | cauldron/CauldronSeeder.sol:850 | registry | NONE | fresh |
| `CauldronSeeder.rescue` | cauldron/CauldronSeeder.sol:888 | registry | sends loose ERC20 through `transfer` (line 891) a… | fresh |
| `LaunchSniper.launch` | cauldron/LaunchSniper.sol:69 | owner | forwards `msg.value` through `play` (line 92) and… | fresh |
| `LaunchSniper.sweep` | cauldron/LaunchSniper.sol:102 | owner | sends native through `call` (line 104) or ERC20 t… | fresh |
| `MigrationVesting.startVest` | cauldron/MigrationVesting.sol:168 | anyone | NONE | fresh |
| `MigrationVesting.vestBatch` | cauldron/MigrationVesting.sol:191 | anyone | NONE | fresh |
| `MigrationVesting.claim` | cauldron/MigrationVesting.sol:249 | anyone | NONE | fresh |
| `MigrationVesting.claimFor` | cauldron/MigrationVesting.sol:256 | anyone | NONE | fresh |
| `MigrationVesting.setVestWindow` | cauldron/MigrationVesting.sol:352 | owner | NONE | fresh |
| `MigrationVesting.setStakerOracle` | cauldron/MigrationVesting.sol:359 | owner | NONE | fresh |

## art

Files: `cauldron/CauldronArtAdapter.sol`, `render/FrenRenderer.sol`, `render/LiquidatoorRenderer.sol`, `render/SSTORE2.sol`, `render/TraitStorage.sol`, `deploy/BadgeArtLib.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `LiquidatoorRenderer.transferOwnership` | render/LiquidatoorRenderer.sol:48 | owner | NONE | fresh |
| `LiquidatoorRenderer.setArt` | render/LiquidatoorRenderer.sol:61 | owner | NONE | fresh |
| `LiquidatoorRenderer.appendArt` | render/LiquidatoorRenderer.sol:80 | owner | NONE | fresh |
| `TraitStorage.storePalette` | render/TraitStorage.sol:57 | owner | NONE | fresh |
| `TraitStorage.storeTrait` | render/TraitStorage.sol:64 | owner | NONE | fresh |
| `TraitStorage.storeTraits` | render/TraitStorage.sol:71 | owner | NONE | fresh |
| `TraitStorage.freeze` | render/TraitStorage.sol:83 | owner | NONE | fresh |

## deploy

Files: `deploy/DeployCauldron.s.sol`, `deploy/DeployLaunchSniper.s.sol`, `deploy/DeployLaunchpad.s.sol`, `deploy/DeployMigrationVesting.s.sol`, `deploy/DeployPerp.s.sol`, `deploy/DeployQuoteAssets.s.sol`, `deploy/DeployRenderer.s.sol`, `deploy/DeployRotationStack.s.sol`, `deploy/DeployV4Core.s.sol`, `deploy/FixFactoryWiring.s.sol`, `deploy/SellVolume.s.sol`, `deploy/SnipeBuy.s.sol`, `deploy/SwapVolume.s.sol`, `deploy/TopUpVenue.s.sol`

| function | where | who can call | value | map |
|---|---|---|---|---|
| `DeployCauldron (declared in DeployCauldron.s.sol).run` | deploy/DeployCauldron.s.sol:45 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployLaunchSniper (declared in DeployLaunchSniper.s.sol).r…` | deploy/DeployLaunchSniper.s.sol:28 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployLaunchpad (declared in DeployLaunchpad.s.sol).run` | deploy/DeployLaunchpad.s.sol:114 | caller satisfying the in-body msg.sender check | NONE | stale |
| `DeployMigrationVesting (declared in DeployMigrationVesting.…` | deploy/DeployMigrationVesting.s.sol:54 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployPerp (declared in DeployPerp.s.sol).run` | deploy/DeployPerp.s.sol:65 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployQuoteAssets (declared in DeployQuoteAssets.s.sol).run` | deploy/DeployQuoteAssets.s.sol:29 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployRenderer (declared in DeployRenderer.s.sol).run` | deploy/DeployRenderer.s.sol:38 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `DeployRotationStack (declared in DeployRotationStack.s.sol)…` | deploy/DeployRotationStack.s.sol:111 | caller satisfying the in-body msg.sender check | transfers token or native value through `transfer… | fresh |
| `DeployV4Core (declared in DeployV4Core.s.sol).run` | deploy/DeployV4Core.s.sol:55 | caller satisfying the in-body msg.sender check | NONE | fresh |
| `FixFactoryWiring (declared in FixFactoryWiring.s.sol).run` | deploy/FixFactoryWiring.s.sol:33 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `RecoverVenue (declared in TopUpVenue.s.sol).run` | deploy/TopUpVenue.s.sol:102 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `SellVolume (declared in SellVolume.s.sol).run` | deploy/SellVolume.s.sol:24 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `SnipeBuy (declared in SnipeBuy.s.sol).run` | deploy/SnipeBuy.s.sol:50 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `SwapVolume (declared in SwapVolume.s.sol).run` | deploy/SwapVolume.s.sol:35 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `TopUpVenue (declared in TopUpVenue.s.sol).run` | deploy/TopUpVenue.s.sol:55 | off-chain script invoker; broadcast authority comes from configured k… | NONE | fresh |
| `VenueSeeder (declared in DeployRotationStack.s.sol).seed` | deploy/DeployRotationStack.s.sol:271 | caller satisfying the in-body msg.sender check | NONE | fresh |
| `VenueSeeder (declared in DeployRotationStack.s.sol).seedBand` | deploy/DeployRotationStack.s.sol:329 | caller satisfying the in-body msg.sender check | NONE | fresh |
| `VenueSeeder (declared in DeployRotationStack.s.sol).recover` | deploy/DeployRotationStack.s.sol:419 | caller satisfying the in-body msg.sender check | transfers token or native value through `transfer… | fresh |
