# Compatibility testing plan

This is a plan for separately authorized testing. No builds, node execution,
wallet recovery, new tests, or production transactions were performed by the
documentation handover. See the [known test limitations](maintenance-notes.md#build-and-test-status).
Passing inherited regtest tests alone cannot establish SOH mainnet compatibility.

## Baseline and fixture custody

Record the baseline source commit, binary checksums, build inputs/options,
platform, command line, and intended change. Distinguish version strings from
verified source provenance. Obtain independently agreed chain data with genesis,
existing checkpoints, a common recent height/hash, chainwork, and a manifest.
Preserve differing histories rather than selecting one by unexplained override.

Keep full original archives immutable and run each experiment on a separate copy.
Use synthetic keys or explicitly authorized private fixtures with controlled
custody. Never upload real wallet backups, mnemonics, private descriptors, keys,
passwords, or RPC cookies to CI or public issues. Disable P2P/network access in
historical binary tests and use a separate environment when networking is needed.

## Required checks

| Area | Cases and required evidence |
| --- | --- |
| Build and packaging | Linux, macOS Intel/ARM, Windows; exact inputs, options, installed filenames, notices, hashes, independent reproducibility where available |
| Genesis/identity | All configured genesis header IDs and Scrypt digests; reject wrong magic, wrong chain and wrong backend genesis |
| Complete history | Replay preserved mainnet from genesis with full validation; compare checkpoint hashes, accepted blocks and UTXO state at agreed heights |
| Difficulty | Blocks around parent boundary 103,000 and candidate boundary 104,950; windows, clamps, rounding, overflow handling, reversed/equal timestamps and LWMA's 60-block weighting |
| Issuance | Before/at/after 180,000; one-output destination restriction, reward plus fees, halving boundaries 200,000/400,000 and later shifts, maturity boundary 20 |
| Timestamp and deployment | Height 191,000 time allowance; median time and host time; BIP9 state from historical headers, including Taproot's minimum height 709,632 |
| Reorganization | Rollbacks of 9/10/11 blocks, heights around 263,000, direct extensions, node restart, IBD versus synchronized state, differing arrival order and partition recovery |
| Header synchronization | Fresh start, old fork/low-work headers, partial-download restart, and inherited difficulty-transition checks; confirm both valid history acceptance and resource bounds |
| Historical releases | Known differing issuance heights and P2SH prefixes; record expected incompatibilities rather than requiring every old binary to accept current mainnet |
| Wallet databases | Non-HD/HD BDB, BDB 4.8 compatibility, encrypted backups, SQLite descriptors, imported keys/scripts, multisig and watch-only wallets |
| Recovery | Legacy external/change paths, descriptor origins/ranges, compressed/uncompressed keys, historical prefix-5 P2SH, large gaps, full scans versus birthday scans, and all known change outputs |
| Transactions | Legacy/witness-v0 parsing and serialization, txid/wtxid, sighashes/signatures, locktime/sequence, prevout authentication, malformed encodings, amount bounds and wrong-network addresses |
| Fees and UTXOs | Actual signed virtual size, dust by script type, zero/negative backend estimates, bounded fallbacks, sweep/subtract-fee/change cases, immature/locked/conflicted outputs |
| Messages | Golden compact P2PKH signatures with the unchanged Bitcoin signed-message prefix; reject altered prefixes and unsupported address types |
| Electrum/mobile | Backend method/protocol capabilities, wrong-genesis failure propagation, missing/forged history or outputs, proofs, duplicate/pending transactions and cache invalidation after reorg |
| Restoration/operations | Fresh archive restore, index rebuild, one endpoint unavailable, loss of old seeds, certificate expiry, log/disk limits, operator access loss and backup ownership |

Use public historical transaction/block bytes as golden vectors where possible.
Assert exact serialization/hashes; for ECDSA signatures verify sighash and validity
as well as any deterministic byte expectations supported by the signer. Test
synthetic spends without broadcasting; isolated `testmempoolaccept` checks are
useful but do not replace block validation or historical replay.

Derive test expectations from preserved chain data and independently reviewed
rules, not by copying the implementation under test. For wallet recovery, verify
known used addresses, redeem scripts, historical transactions, balances, and spent
outputs before accepting a result. A zero balance or a successful wallet open is
not sufficient evidence.

## Full validation and reindex distinctions

On isolated copies, distinguish fresh block import/full validation from
`-reindex-chainstate`, reused chainstate databases, and restored indexes. They do
not execute identical paths. Record assume-valid/minimum-work settings and actual
effective configuration; do not alter consensus to make replay pass.

The existing source's mainnet assume-valid and minimum-chain-work constants are
zero, but command-line options can override behavior. Document reorg/IBD state
and block arrival order because they can affect chain selection. A matching tip
hash does not replace signature, UTXO, and wallet recovery checks.

## Results and decision record

For each case retain: fixture identity/hash and access classification; source and
binary identity; platform; exact command/configuration; expected versus observed
result; sanitized logs; reviewer; and pass/fail/not-run status. Keep private
fixture records separate from public reports.

Expected historical failures and inherited test defects must be listed explicitly.
Unexplained divergence in accepted blocks, PoW, transaction encoding/signing,
UTXOs, or wallet recovery blocks a compatibility claim. Preserve the failure
evidence and investigate it; do not update checkpoints, rules, golden data, or
wallet formats to suppress a mismatch. Any fix or production rollout needs a
separate reviewed proposal.
