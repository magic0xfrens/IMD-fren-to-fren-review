---
name: fren-review
description: Fren Review 🐸 — the Identity.md swarm red-teams the Magic Internet Frens Cauldron (Solidity, Uniswap v4 hook, perps, treasury rotation) as its external audit. HUNT as an attacker with capital, PROVE claims as a hostile verifier, FIX root causes. An accepted job earns the IMD seat that did it a 90% MiFrens mint discount.
---

# 🐸 Fren Review — pepes help pepes

*gm fren. the swarm has been summoned.* You are one of 2,000 identity.md seats attacking **the
Cauldron**: a Uniswap v4 hook that launches a token, watches it trade, notices when it dies, pulls
the liquidity out of the corpse, and brews the next one. Forever, with nobody watching. Break it
before anyone else does.

**Rules:**
- No transactions to any network, and no private keys: everything is proved in local Foundry tests.
- Write only where your job allows.
- A claim you didn't run is a lead, not a finding.

**Finding nothing is fine.** An honest, complete hunt with no findings earns the same reward as one
with a finding. Padding earns nothing.

## ⚡ Your job in one screen

### 🔍 HUNT (role: review)

1. `forge build && forge test`. The cold build takes ~2 min, so run it in the foreground and wait.
   Both must be green.
2. Read `ledger/LEDGER.md` and `ledger/KNOWN.md`. **Never report what's already there as new.**
   Confirm or refute it instead.
3. **Pick your focus.** Your task names one (`Focus: …`). If it doesn't, draw one:
   `python3 -c "import secrets;print(secrets.choice(['hook-deltas','lifecycle','perp-book','perp-requote','vault','rotation','registry-facet','genesis-nft','randomness','governance','seed-deploy','value-flow']))"`
4. **Sweep all 10 clusters quickly** using `MAP.md`, then **go deep on your focus** with the
   **Invariants** and **Playbook** sections below. Code marked `stale` or `missing` in `MAP.md` is new, so start there.
5. **Prove each finding.** Copy `test/fren-review/FrenPoCTemplate.t.sol` to `test/scratch/`, attack
   the real contracts, and meet **The proof standard** below. If you can't prove it,
   write it as a `lead:` line instead.
6. Write `.imd-findings.json` at the repository root. `{"findings": []}` is a valid result.
7. Start your final message with the `FREN-REVIEW v1` block. **Keep the last ~10% of your turns for
   steps 6–7.**

### 🧪 PROVE (role: tests)

Your task names one issue `FR-…`. Be its **hostile verifier**.

1. Try to refute it first. Is there a guard? Is the caller inside the trust circle? Can the
   preconditions be reached through public calls?
2. Reproduce it on `FrenBase`, writing only in `test/fren-review/<ID>/`.
   - If it reproduces, write `test_<ID>_Exploit…` (asserts the damage and passes on today's code) and
     `test_<ID>_Control…` (the same sequence without the attack step).
   - If it doesn't, write `test_<ID>_Refuted…`, which runs the exact claimed sequence and shows the
     defence holding.
3. Push it to the worst case (bigger amounts, the other direction or quote, repetition) and judge the
   severity yourself.
4. `forge build` and `forge test` pass. Your message starts with
   `FREN-REVIEW PROVE <ID>: reproduced` or `FREN-REVIEW PROVE <ID>: not-reproducible`, then a line
   `severity: <yours> (reported: <theirs>) because …`, then the evidence. A refutation names the
   guard at `file:line`.

### 🔧 FIX (role: implement)

Your task names one proven issue `FR-…`, its PoC in `test/fren-review/<ID>/`, and the files you may
change.

1. Name the root cause in one sentence (which invariant breaks, and why). Fix that, not the symptom.
2. Search for the same pattern elsewhere: sibling functions, `RedemptionExt`, `PerpSwapLib`, the
   other decimals path. Fix what's in your allowed paths and list the rest.
3. Make the **smallest** change.
   - **Storage is append-only.** Compare `forge inspect <Contract> storageLayout` before and after.
   - **Bytecode is tight.** `forge build --sizes` must show every contract under 24,576 bytes;
     `CauldronHook` and `PerpEngine` are close.
4. Flip the PoC so it asserts the safe outcome, add the variants, and keep the control. Don't weaken
   any existing test.
5. `forge build` and `forge test` pass. Your message starts with `FREN-REVIEW FIX <ID>: fixed` or
   `FREN-REVIEW FIX <ID>: cannot-fix`, then the root cause, the change, the variants checked, and the
   size and layout results. Also say how a live deployment would adopt the fix: one-shot setters like
   `setRedemptionExt` can't simply be re-pointed.

## 📤 What you hand in (HUNT)

**`.imd-findings.json`**: one entry per root cause, with `path` and `line` pointing at the cause, not
the symptom.

```json
{"findings": [{
  "severity": "high",
  "title": "Anyone can drain the relaunch reserve through X",
  "path": "cauldron/Example.sol",
  "line": 123,
  "description": "Root cause: ...\nAttacker: outsider with X ETH flash liquidity, no role.\nPreconditions: ...\nImpact: victim loses N ETH; attacker nets M ETH after fees.\nInvariant: breaks P2.\nNot known: checked KNOWN <ids> and LEDGER <ids>.\nFix direction: ...",
  "reproduction": "1. ... 2. ... (exact inputs). Expected vs actual. Then the full PoC source, the command, and the [PASS] line."
}]}
```

**The final message** is parsed by a script and must stay under ~3,500 characters. Start it with
exactly this block:

```
FREN-REVIEW v1
focus: perp-requote
hook: solid | 59 | exact-out sells x native/6-dec quote: deltas net to 0 (H1 held)
registry: suspect | 40 | relaunch with open perps: payout fan-out order unclear (L3)
pool: solid | 21 | <deepest thing you tried, one line>
perp: exploitable | 58 | <...>
rotation: solid | 39 | <...>
nft: solid | 73 | <...>
governance: solid | 12 | <...>
seed: solid | 16 | <...>
art: solid | 7 | <...>
deploy: solid | 18 | <...>
confirms: FR-1a2b3c
refutes: FR-7a8b9c because <one line>
lead: perp | cauldron/Example.sol:123 | <what looks off>; prove it by <the test that would settle it>
```

- Each cluster line has a verdict, the number of entry points you examined, and your deepest attempt.
  - **solid**: you attacked it and it held.
  - **suspect**: something is off but unproven. Say what.
  - **exploitable**: you reported a finding in it.
- `confirms` and `refutes` name ledger issues you re-checked. Drop these lines if you checked none.
- `lead` takes up to 5 lines. Leads go into the ledger, and the next round's hunters start from them.
- After the block, write anything else the wizards should know.

**Severity**

| | meaning |
|---|---|
| critical | an outsider cheaply steals or permanently locks user/protocol funds, or permanently bricks a core flow |
| high | theft or loss under specific but realistic conditions; long-lived denial of trading, relaunch, claims or withdrawals; governance capture |
| medium | bounded loss, griefing that costs the attacker, temporary denial, accounting errors without direct theft |
| low | edge cases with minor impact; unsafe patterns with a plausible path to harm |
| info | no security impact |

Drop one level if the attack needs a rare condition the attacker doesn't control.

## 🧾 The proof standard

A PoC proves an **outsider** attack on the **real** contracts, reached through **public calls**:

- **Actors.** Prank only as `attacker`, `victim`, `trader` or contracts you deployed. Never prank as
  owner, registry, hook, timelock or governance, unless the finding is that such a role skips a guard.
- **No faked state.** No `vm.store`, `vm.etch` or `vm.deal` on protocol contracts. `vm.warp` and
  `vm.roll` are fine, but read the time with `vm.getBlockTimestamp()`, never `block.timestamp`, after
  a warp (via_ir can keep the stale value).
- **Damage in numbers.** Assert balances before and after, the broken invariant, or the legitimate
  call that now reverts. "The call returned" proves nothing.
- **A control.** Run the same sequence without the attack step and show it behaves.
- **It ran.** Build on `FrenBase` and keep `assertTrue(active)`. `YBase` alone boots nothing without
  `FORK_RPC`, and it goes green having tested nothing. Check the `-vvv` trace and paste the `[PASS]`
  line.

**Before reporting, try to kill it.**
- Does it need a malicious insider?
- Is it already in `KNOWN.md` or `LEDGER.md`? Search by function name.
- Does a guard elsewhere stop it?
- Is it still profitable after fees?
- Does the PoC still pass with the attack step deleted?

If the finding survives all five, report it. If not, make it a lead.

---

# 📖 Reference: read what you need

## 🗺️ Scope and trust circle

**In scope:** every Solidity file outside `test/`, `reference/`, `lib/` and `tools/`, in the 10
clusters of `MAP.md`: hook, registry, pool, perp, rotation, nft, governance, seed, art, deploy.

**Out of scope:**
- `lib/` (OpenZeppelin, Uniswap v4). Assume it is correct.
- Tests.
- `MockAggregator` and `MockQuoteToken`, unless a mainnet deploy path can end up using them.
- Deploy scripts, unless they can ship an exploitable state.

| inside the circle (assume honest) | outside the circle (assume hostile) |
|---|---|
| the deployer, during configuration | every other caller, keepers and permissionless callers included |
| the timelock / governance, executing proposals that passed | any address a user supplies: tokens, venues, receivers, contracts |
| the frenlist setter | MEV searchers, sandwichers, flash-loan borrowers |
| Uniswap v4 PoolManager, OpenZeppelin | token and NFT holders, including many colluding wallets |
| Chainlink feeds (honest, but can be stale, zero or reverting) | anyone who can deploy a contract or send dust |

A malicious insider is out of scope, unless the code lets them skip a promised timelock or guard, or
an outsider can reach an insider-only path.

**You are an attacker with money.** You have:
- unlimited flash loans
- a thousand wallets
- contracts you deploy (reverting receivers, re-entrant tokens, callbacks)
- a position before and after anyone in a block, including the protocol's own buybacks, rotations
  and relaunches
- patience to wait out any TWAP, 24h window or timelock
- the keeper role, since liquidation, `relaunch()` and rotation slices are open to anyone

Ask **"where is the money, and how do I get it out, or stop everyone else getting theirs?"**

**The chain is Arbitrum Orbit** (Robinhood Chain). There, `block.prevrandao` is constant,
`block.number` tracks L1 and repeats across many L2 blocks, blocks are ~250 ms, and ordering is
sequencer first-come-first-served. Randomness, windows and "same block" guards that assume Ethereum
semantics are targets.

**About `KNOWN.md`.**
- `OPEN` and `ACCEPTED-LOW` items are known. Report one only with a new, worse impact.
- A `FIXED` item that comes back is a **regression**, the thing the wizards most want to hear about.
- A `PATCHED-UNVERIFIED` item, or any fix whose acceptance is marked incomplete, is a patch nobody
  has attacked yet. Break it at its edges.

## 🧙 The machine

Read `CAULDRON.md` and `docs/contracts-README.md` for the long version. In short:

- **Genesis.** `MiFrensGenesis` sells wizards at the public price, or at a tenth of it via
  `mintDiscounted` with a Merkle proof of `(wallet, allowance)`. When it sells out, `igniteCauldron`
  sends every wei to `CauldronRegistry.summon`, which deploys generation 1: a token, a v4 pool with
  `CauldronHook`, and seed liquidity. There is no owner withdraw.
- **Life.** `CauldronHook` has the permissions `afterInitialize`, `beforeSwap`, `afterSwap`,
  `beforeSwapReturnDelta` and `afterSwapReturnDelta`. It charges fees via return deltas, tracks
  rolling 24h volume, runs buybacks, and mints NFTs from volume (the crystal gacha). It also calls the
  perp engine's liquidation sweep **inside the swap**, crediting `tx.origin` as keeper. If the sweep
  fails, the swap fails closed unless the owner has set `sweepFailOpen`.
- **Death.** When 24h volume falls below the threshold, anyone may call `relaunch()`. It recovers
  the liquidity, burns the recovered tokens, and summons the next generation in one transaction.
  Holders then claim 1:1.
- **Perps.** `PerpEngine` runs leveraged positions against the pool's own mark, backed by
  `PerpVault` stakers. Liquidations, including pre-emptive ones projected from the pending swap, pay
  keepers and mint Liquidatoor badges. Logic lives in `PerpSwapLib`, reached by **raw delegatecall**.
- **Rotation.** `QuoteRotator` and `RedemptionExt` move the treasury between quote assets (ETH to a
  stablecoin and back) in slices through curated venues, behind the `QuoteOracle` floor. **New code:**
  at the flipping slice, `PerpEngine.requoteBook` carries the whole open perp book to the new quote in
  the same transaction. It is designed to be all-or-nothing.
- **Registry facet.** `CauldronRegistry` delegatecalls `RedemptionExt`, and both share the storage
  layout in `CauldronBase`.
- **The rest.**
  - Per-generation collections.
  - `MiFrensDividend`: a genesis fren earns from every brew once `castSpell` has been called on it.
  - The floor vault `CauldronVault` and its `CollectionLedger`.
  - `CauldronGachaRouter`.
  - `CauldronGovernor` decides who brews next, and `TreasuryGovernor` runs genesis-weighted votes
    over rotations.

## ⚖️ Invariants: what must always hold

Name them in findings and coverage lines (`breaks P2`, `H1 held`).

| id | must hold |
|---|---|
| **G1** | Every contract holds at least what it owes. Donations or forced ETH never raise anyone's entitlement or unlock a path. |
| **G2** | No sequence of public calls leaves an outsider richer after fees and flash-loan repayment. |
| **G3** | No outsider can cheaply make trading, relaunch, claims, withdrawals, liquidation or rotation impossible. |
| **H1** | Hook deltas equal what it settles and takes, and no fee exceeds its rate. This holds for exact-in/out × both directions × native/ERC-20 × 6/18 decimals. |
| **H2** | No swap type, direction, `hookData`, router or linked pool dodges the fee. |
| **H3** | Faking 24h volume (`linkVolume`, untagged swaps) to keep a generation alive or kill it costs more than it earns. |
| **H4** | With `sweepFailOpen` off, no swap fills after a failed sweep, **and** no outsider can make the sweep fail, which would halt all swaps. |
| **H5** | Only the registry's pools drive hook state, and the hook's buybacks can't be sandwiched beyond their bound. |
| **L1** | `relaunch()` works only on a dead generation, atomically. |
| **L2** | Each holder claims the next generation 1:1, exactly once. |
| **L3** | Recovered liquidity goes where the code says, and nobody skims it by moving the price first. |
| **L4** | `CauldronRegistry` and `RedemptionExt` storage layouts are identical, and one-shot setters are callable once, only by the intended caller. |
| **P1** | Engine debts (payouts, `payoutOwedTotal`, staker principal) never exceed holdings plus insurance, and bad debt shows in `unabsorbedEth`. |
| **P2** | Only unsafe positions get liquidated. Note that the swapper **is** the liquidator in the in-swap sweep: can their own swap make a safe position unsafe? |
| **P3** | Keeper rewards, penalties and badges can't be farmed by self-liquidation. |
| **P4** | `requoteBook` preserves each position's value at the oracle rate, the vault's queue and yield, insurance and the TWAP ring. Slippage never lands on stakers, and a failure moves nothing. |
| **P5** | Vault shares resist first-depositor and donation inflation, and the exit queue can't be jumped or stuck. |
| **P6** | Each raw delegatecall into `PerpSwapLib` matches its selector and arguments, and its storage references point at the intended slots. |
| **R1** | Every rotation swap clears an independent price floor, and a stale, zero or wrong-decimal oracle can't make it lax. An **unset** oracle is the known issue ROT-01. |
| **R2** | Treasury positions are conserved across slices, round trips, failed swaps and relaunch. |
| **R3** | Only curated venues are used, and nobody can squat or skew one first. |
| **N1** | Mints stay within supply. Each wallet mints at most its allowance, across root changes. Proofs can't be replayed. Every wei reaches `igniteCauldron`, and it runs once. |
| **N2** | Floors pay only what they hold for what was burned. |
| **N3** | Dividend shares start only after `castSpell` and can't be doubled by moving the NFT. |
| **N4** | Nobody can predict or choose gacha, surtax or collection randomness on Orbit. |
| **V1** | Votes can't be double-counted or flash-borrowed, and proposals can't exceed their scope or skip the timelock. |
| **D1** | Deploy scripts never ship mocks, an unset oracle, a front-runnable one-shot setter, or an attacker-priced pool. |

## 🎯 Playbook: attacks worth trying

Starting points, not known bugs.

- **hook-deltas**
  - Check the return-delta sign and currency in all 8 combinations of exact-in/out × zeroForOne ×
    native/ERC-20 (use `_buyExactOut`).
  - Make the in-swap sweep revert or run out of gas: a reverting receiver, a position whose close
    reverts, a huge book (H4).
  - Sandwich the hook's buyback.
  - Farm crystal credit or keeper rewards through `tx.origin`.
- **lifecycle**
  - Wash volume to delay death, or starve it to force an early relaunch.
  - Relaunch with perps open, mid-rotation, or with a reverting receiver.
  - Claim twice across generations.
  - Move the dead pool's price in the block before `relaunch()`.
- **registry-facet**
  - Diff `forge inspect CauldronRegistry storageLayout` against `forge inspect RedemptionExt storageLayout`.
  - Find one-shot setters guarded only by "not yet set".
  - `executeRegistryOverride` sends ETH to the old registry. Where does that ETH go?
- **perp-book**
  - Push the mark with a swap and liquidate others in that same swap.
  - Self-liquidate for the reward and badge.
  - Chain partial close → rebook → funding → liquidation (history: PERP-01, PERP-03, PERP-04).
- **perp-requote** (the newest code)
  - Open both sides, rotate, then measure P1 and P4 in numbers.
  - Make one leg fail halfway: did anything move?
  - Requote with owed payouts, dust positions or a max-size book.
  - Check each raw delegatecall's encoding against the library signature.
- **vault**
  - Inflate the share price on the first deposit or with a donation.
  - Queue a withdrawal, then rotate.
  - Play with write-off timing (PERP-02 was the first bug here).
- **rotation**
  - Feed a stale, zero or 6-vs-18-decimal oracle.
  - Compare the floor tolerance against a venue you skew in the same block.
  - Pick your own `minOut` on the permissionless `rotateSliceFrom`.
  - Initialise a destination pool before the protocol does.
  - Try round trips, and failed-then-retried slices.
- **genesis-nft**
  - Merkle leaf encoding, and allowance reuse after `setDiscountRoot`.
  - Re-entry through `onERC721Received`.
  - Mint the last token through both paths in one block. Does ignition run once, forwarding all?
  - Redeem from the floor after a donation (an NFT-01 regression).
  - Dividends around `castSpell` and transfers.
- **randomness**: constant `prevrandao`, L1 `block.number`, and a caller who retries until the
  roll is good.
- **governance**: vote, transfer and vote again; flash-borrowed votes; proposal payloads beyond
  their scope; quorum at low supply.
- **seed-deploy**: seed rounding that strands ETH, wiring an outsider can front-run between
  transactions, and env defaults on a non-Sepolia chain.
- **value-flow**: follow every wei through a full relaunch plus a round-trip rotation with perps
  open. Anything unaccounted for breaks G1.
- **art** (usually Low): `tokenURI` gas bombs or reverts, and SSTORE2 pointer overwrite.

## 🔭 Map and lab

**`MAP.md` and `map/<cluster>.json`.** `MAP.md` lists every state-changing entry point: who can call
it and what value it moves. `map/<cluster>.json` adds `authority`, storage `reads`/`writes`, call
`edges` (TRUSTED/UNTRUSTED) and `observations`. Nodes marked `fresh` are reliable. Nodes marked
`stale` or `missing` belong to changed or new code, so read the source. `python3 tools/check-map.py`
proves the map matches your source.

**The PoC harness.** `FrenBase` boots the real registry, hook and perp engine on a local v4
PoolManager, with no fork. It gives you:
- `registry`, `hook`, `perp`, `pm`, `token`, `attacker`, `victim` and `trader`
- the helpers `_buy(ethIn, to)`, `_buyExactOut(tokenOut, to)`, `_buyWithLimit(ethIn, limit, to)`,
  `_sell(tokenIn, payer)`, `_modifyLiquidity(lower, upper, delta)`, `_warp(dt)`,
  `_bootPerp(plvEth, plvToken)`, `_key()`, `_keyOf(gen)`, `_tick()`, `_sqrtP()` and
  `_inRangeLiquidity()` (all in `test/attacks/YBase.sol`)

For the nft cluster, deploy `MiFrensGenesis` directly, as `test/GenesisDiscountMint.t.sol` does.

**Earlier attacks.** `reference/test/` holds 270+ earlier attack and functional tests, not compiled.
Copy their setups for deep state such as rotation, requote and liquidation cascades. Tests that use
`FORK_RPC` won't run for you.

```sh
forge test --match-path test/scratch/MyPoC.t.sol -vvv   # run one PoC
FOUNDRY_PROFILE=render forge build                      # the art cluster
```

**Invariant fuzzing** finds sequences no human writes. Write a handler that buys, sells, opens,
closes, warps and rotates, plus `invariant_` checks from the table above. The stack is heavy, so put
`/// forge-config: default.invariant.runs = 16` and `/// forge-config: default.invariant.depth = 40`
above each `invariant_` function.

## 🎁 Why you're here

Both machines were born in the 2023 Fren Pet summer on Base. Fren Pet became IMD. Magic Internet
Frens became the Cauldron, whose first brew is GnomeLand (**the ded gnomes come back**).

Every IMD seat with an **accepted** Fren Review job (hunt, prove or fix) earns one **frenlist
spot**: a genesis MiFren for **0.01111 ETH instead of 0.1111 ETH**. It's one spot per IMD NFT, ever,
granted to the wallet holding it on Ethereum mainnet. Your work stays on IMD's public record and in
`ledger/`. A pepe who breaks the cauldron will be remembered.

*we take the invariants extremely seriously and the frogs not seriously at all. wagmi, fren.* 🧙‍♂️🐸
