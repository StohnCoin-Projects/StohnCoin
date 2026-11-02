# SOH wallet compatibility and mobile integration reference

This describes the [audited source](maintenance-notes.md), not a new wallet or
an approved implementation. Native mobile use transfers SOH on its own chain;
it does not require bridging. Existing Core wallets must remain recoverable.

## Address and private-key formats

See [`chainparams.cpp`](../src/kernel/chainparams.cpp) and
[`key_io.cpp`](../src/key_io.cpp). Validate complete encodings and checksums;
do not validate by the first visible character alone.

| Field | Mainnet | Testnet / regtest |
| --- | --- | --- |
| Atomic units per SOH | 100,000,000 | 100,000,000 |
| P2PKH version | 64 (`40` hex) | 111 (`6f`) |
| Current P2SH version | 63 (`3f`) | 196 (`c4`) |
| WIF version | 191 (`bf`) | 239 (`ef`) |
| Extended public version | `0488b21e` | `043587cf` |
| Extended private version | `0488ade4` | `04358394` |
| Witness HRP | `soh` | `tsoh` / `rsoh` |

Base58 addresses use Base58Check. WIF contains the network version, a valid
32-byte secp256k1 private scalar, and optionally `01` for compressed public keys,
followed by the checksum. Preserve compressed/uncompressed status during recovery.
Extended-key prefixes overlap Bitcoin's; an `xpub`/`xprv` alone does not identify
the network or complete derivation scheme.

Witness version zero uses Bech32; version one uses Bech32m. Mainnet's Taproot
minimum activation height is 709,632, with other BIP9 conditions also required.
Do not offer Taproot receiving merely because the encoder accepts it. Read
`getdeploymentinfo` and test rule enforcement before relying on activation.
The current Core wallet default receiving type is `legacy` (P2PKH).

Historical tags through `v3.0.1` used P2SH version 5; `v3.0.2` and later use 63.
The same script hash can refer to an existing SOH output despite a changed displayed
address. Explicit historical SOH recovery must preserve the redeem script and
verify the underlying output. Do not silently accept an ambiguous Bitcoin-style
address as an ordinary SOH destination or invent new prefixes.

## Derivation and backup formats

[`scriptpubkeyman.cpp`](../src/wallet/scriptpubkeyman.cpp) implements:

| Wallet | Existing derivation / recovery material |
| --- | --- |
| Non-HD legacy | Individual keys, imported scripts, and a complete wallet backup |
| Legacy HD | External `m/0'/0'/i'`; internal `m/0'/1'/i'` when HD split is supported |
| Descriptor | Actual descriptors, key origins, ranges, next index, and change designation |

Current mainnet descriptor generation uses `m/purpose'/0'/0'/change/index`, with
purpose 44, 49, 84, or 86 for the corresponding output type. Test networks use
coin type `1'`. Core does not expose a BIP39 mnemonic merely because it uses HD
keys. A mnemonic from another wallet must not be assumed to reconstruct a Core
wallet, and a single seed does not recover unrelated imported keys/scripts.

Any future mobile mnemonic/path convention needs explicit documentation, test
vectors, and a permanent recovery contract. No new coin type or derivation default
is assigned by these documents. Retain separate recovery support for existing
Core paths. Check the current SLIP-44 registry before claiming a registered SOH
coin type; the October 31, 2026 assessment found no Stohn/SOH entry.

## Backup and recovery

1. Identify the wallet version/type, encryption, original application, network,
   derivation metadata, imported keys/scripts, and any multisig participants.
   Keep the original backup immutable and offline; use a copy for each attempt.
2. Use the wallet-aware `backupwallet` RPC for a loaded wallet, with a private
   destination accessible to the daemon. Alternatively preserve a complete,
   consistently shut-down wallet directory. Do not treat a raw live database copy
   as a proven backup. Preserve Berkeley DB 4.8 compatibility for legacy wallets.
3. Restore only in a separate, wallet-enabled compatible installation using a new
   wallet name and data directory. For an offline inspection start with
   `-networkactive=0`; inspect its effective settings and do not broadcast spends.
   `restorewallet` creates/loads a wallet; it is not a read-only diagnostic.
4. For descriptors, preserve origin, range, birthday, and internal/external
   metadata. `listdescriptors` defaults to public descriptors; private exports and
   legacy `dumpwallet` material are secrets. Even public descriptors expose wallet
   history and should not be placed in a public issue or archive.
5. On an isolated validated archive, compare birthday-based scanning with a full
   height-zero scan when history is uncertain. `rescanblockchain` updates wallet
   state and needs the relevant blocks. Current short timestamp buffers can miss
   older history; low gap limits can also omit used addresses and change.
6. Compare known historical transactions, output scripts, balances, and spentness.
   Test signing with synthetic keys before any optional holder-authorized sweep
   to a fresh wallet. An empty initial scan is not proof that funds are gone.

Check the selected binary's `help backupwallet`, `help restorewallet`, and
`help rescanblockchain` for its exact RPC interface. Preserve BDB/SQLite backups,
passwords, and scripts separately under the holder's control. Do not request
private wallet files, keys, or mnemonic phrases in community support channels.

## Transactions, UTXOs, signing, and fees

Serialization remains Bitcoin-family: version, inputs (outpoint, script,
sequence), outputs (integer value, script), locktime, and optional SegWit witness.
Transaction IDs use double SHA-256; witness IDs include witness serialization.
Use secp256k1 ECDSA and the existing legacy/BIP143 sighash rules for established
legacy/witness-v0 outputs. Scrypt is block PoW, not transaction signing.
Taproot signing is conditional on verified activation and compatibility tests.

The legacy message prefix remains `Bitcoin Signed Message:\n` in
[`message.cpp`](../src/util/message.cpp). Keep its serialization exactly; do not
replace it as branding. Core's compact message verification is for P2PKH addresses.

Use integer atomic amounts and authenticate previous transaction bytes, txids,
output amounts, and scripts before signing. Account for change, pending/conflicting
spends, locked outputs, reorgs, and coinbase maturity 20. Coinbase spendability
uses the candidate spending height; test the boundary instead of using Bitcoin's
100-block constant or a generic confirmation-display threshold.

Fees are input sum minus output sum; ordinary virtual size is the ceiling of
weight/4. Core relay policy can increase effective size for signature-operation
cost; see [`GetVirtualTransactionSize`](../src/policy/policy.cpp).
The compiled minimum relay default is 1,000 atomic units per kvB, or one atomic
unit per vbyte. Dust thresholds depend on script type and relay policy. Query the
actual node/backend policy and bound fallback estimates on a quiet network; an
estimate does not guarantee miner acceptance. Verify actual signed size and fee,
including subtract-fee and sweep cases. The 15-confirmation project recommendation
is a risk guideline, not immutable finality.

## Stack Wallet and a lightweight backend

The October 31 assessment inspected Stack Wallet commit
`d6ca375dfc1a86097ebb372962b12fdba4ba63e2`. Its Bitcoin-family architecture uses
Electrum services. Reuse candidates are its `coinlib`/`coinlib_flutter`,
`electrum_adapter`, BIP32/mnemonic libraries, transaction/fee code, and tests.
Review the exact dependency revisions and licenses before implementation.
[Stack Wallet source](https://github.com/cypherstack/stack_wallet/tree/d6ca375dfc1a86097ebb372962b12fdba4ba63e2).
Upstream acceptance and production SOH support have not been established.

The practical first proposal is two TLS Electrum-compatible endpoints, each backed
by an independently operated archival, wallet-disabled Core node over private
RPC. Keep signing on the device, permit custom endpoints, and document that a
server can omit wallet history. Verify returned transaction data and inclusion
proofs where implemented; a TLS Electrum connection alone is not SPV validation.

Implement a dedicated SOH definition in a maintained backend such as
[ElectrumX](https://github.com/spesmilo/electrumx/blob/master/src/electrumx/lib/coins.py).
Use SOH genesis, ports/prefixes, SHA256d block IDs, compatible SegWit parsing,
RPC capabilities, and reorg handling. Bitcoin-specific daemon/index requirements
must not be inherited blindly. Unpruned storage and `txindex=1` are a candidate
backend profile to validate, not settings applied by this documentation.

Validate `server.version`, `server.features`, header subscription/retrieval,
script-hash history/balance/listunspent, raw transactions and Merkle proofs, fee
estimates, and broadcasting. Electrum script hashes are reversed SHA-256 of output
scripts. Select a protocol version whose methods the backend actually supports.
Require wrong-genesis detection to stop operation; the inspected shared Stack
routine catches/logs a genesis mismatch, so its error propagation needs testing.
The project's public read-only RPC wrapper is not an Electrum server or broadcaster.

Direct peer SPV would require additional SOH discovery, Scrypt PoW, exact historical
difficulty and chain-selection handling, headers/work verification, and transaction
discovery. Core's compact-filter indexing/serving and bloom serving are disabled
by default. Do not import Litecoin MWEB or Dogecoin AuxPoW assumptions.

Verified header/block archives and wallet birthday heights can accelerate syncing.
Headers are about 80 bytes each before overhead. Mainnet has no configured
assumeUTXO anchor; a copied chainstate is not a trustless snapshot. Test both fresh
indexing and restoration from authenticated archives. See the
[compatibility plan](compatibility-testing.md) before proposing implementation.
