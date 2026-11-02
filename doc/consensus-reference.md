# Existing SOH consensus and network reference

Baseline: `87cb7eed35560c894a6129c93e2e2853e454c8c9` (`v26.x`), assessed October
31, 2026. This records existing behavior; it proposes no rule or checkpoint change.
It is not a replacement specification for all inherited Bitcoin script/validation
rules. Preserve the implementation and verify historical behavior before reuse.

## Authoritative source locations

| Area | Implementation |
| --- | --- |
| Networks, genesis, activations, checkpoints | [chainparams.cpp](../src/kernel/chainparams.cpp) |
| RPC ports and network directories | [chainparamsbase.cpp](../src/chainparamsbase.cpp) |
| Difficulty | [pow.cpp](../src/pow.cpp) |
| Subsidy, special coinbase, chain selection | [validation.cpp](../src/validation.cpp) |
| Timestamp allowance | [chain.h](../src/chain.h) |
| Header serialization / hashes | [block.h](../src/primitives/block.h), [block.cpp](../src/primitives/block.cpp) |
| Amount limits / block limits | [amount.h](../src/consensus/amount.h), [consensus.h](../src/consensus/consensus.h) |

## Network identity

| Parameter | Mainnet | Testnet | Regtest |
| --- | --- | --- | --- |
| P2P magic (wire bytes) | `f74149c4` | `92ea35a1` | `fabfb5da` |
| P2P port | 37218 | 47218 | 19444 |
| RPC port | 32717 | 47217 | 18443 |
| Chain directory beneath base | none | `testnet3/` | `regtest/` |
| Genesis time | 1623334958 | 1623337198 | 1623337779 |
| Genesis nonce | 4221165 | 1115608 | 0 |
| Genesis bits | `1e0ffff0` | `1e0ffff0` | `207fffff` |

Genesis IDs:

```text
main:    c321976e6eab027d1cf712823d37c0995b046f49b53f8dcaf228e7c69c340251
test:    991db00f7a203b32935844b414d98e38437359e6220fdad6e95d8cdbe197b9b5
regtest: 2836239d52eef02caab4f9262c89d9a48ff174792cb1b95b41a642e60f03d7db
```

All three use genesis version 1, reward 100 SOH, and Merkle root
`db105b48fc8898d2edcfea8c8adabf6991d92e33be5ff953bfe235cee3e6b41c`.
The genesis output is not inserted into the spendable UTXO database.
Protocol version is 70020; minimum accepted peer protocol is 31800. These numbers
alone do not establish software identity, security, or agreement on a chain.

## Mainnet proof of work and difficulty

Headers serialize as 80 bytes: version (4), previous block ID (32), Merkle root
(32), time (4), compact target (4), nonce (4). Integer fields use little-endian
encoding; displayed hashes reverse the internal byte order. The block ID is
double SHA-256. The separate PoW digest is Scrypt with the header as password and
salt, N=1024, r=1, p=1, and 32-byte output. Compare its little-endian integer to
the decoded target. Do not use the block ID for this comparison.

Target limit:
`00000fffffffffffffffffffffffffffffffffffffffffffffffffffffffffff`.
Target spacing is 300 seconds. Mainnet permits neither special minimum-difficulty
blocks nor disabled retargeting.

| Stage | Existing selection and calculation |
| --- | --- |
| Initial | Target timespan 600 seconds, interval 2 blocks; timespan clamp 1/4 to 4 times |
| Intermediate | Parent height >=103,000 selects timespan 151,200 seconds / interval 504; clamp 1/4 to 2 times |
| LWMA | Candidate height >=104,950 takes precedence; 60-block window |

Preserve the parent-versus-candidate boundary checks, full-period lookback,
overflow handling, compact-target rounding, and integer operation order. LWMA
forces successive effective timestamps to increase by at least one second, caps
each solve time at 1,800 seconds, weights by position, and divides each target
before summing. Algebraic rearrangement can change integer results. The inherited
low-work-header transition checker does not describe all of this history.

## Mainnet amounts and activation boundaries

| Parameter | Existing value/behavior |
| --- | --- |
| Atomic units per SOH | 100,000,000 |
| Ordinary subsidy | `(100 * COIN) >> (height / 200000)`; zero after 64 halvings |
| Height 180,000 subsidy | 3,982,239 SOH, replacing the ordinary subsidy |
| Special coinbase restriction | Exactly one output, to `STTdqQ1YDtYt7vjFGgnYESeX4SuNuGSFzo` |
| Coinbase maturity | 20 blocks |
| Maximum block weight | 4,000,000 |
| Maximum serialized block size | 4,000,000 bytes |
| Maximum signature-operation cost | 80,000 |
| BIP34, BIP65, BIP66, CSV, SegWit | Mainnet activation heights set to zero |
| Future-time allowance | 7,200 seconds before 191,000; 600 thereafter |
| BIP9 mainnet window / threshold | 8 / 2 |
| Taproot | Bit 2, start 1619222400, timeout 1628640000, minimum height 709,632 |
| Minimum chain work / default assume-valid | Both zero |
| Mainnet assumeUTXO anchors | None |

Block fees are additional to the allowed subsidy. The special coinbase destination
restriction is in the full connection path after the `fJustCheck` return; preserve
and test that distinction. `MAX_MONEY` is 21,000,000 SOH and is a consensus amount
range check, not the total scheduled issuance. Do not change it to match supply
descriptions. Timestamp, median-time, version-bit, and script exceptions in the
source also remain relevant; encoding an address does not activate its rules.

## Checkpoints and reorganizations

The complete mainnet checkpoint table in this baseline is:

| Height | Block ID |
| ---: | --- |
| 0 | `c321976e6eab027d1cf712823d37c0995b046f49b53f8dcaf228e7c69c340251` |
| 103000 | `44b0e4e6f96d562ceafa47eb1dca766b056a163f08b7d68349bbe33763a1d13e` |
| 104950 | `2dbd943d957e26770bc824187ee6bbb3b47273f064063883e895f9a551dbf25a` |
| 180000 | `1a5c53b59ef92f5ceca81467e8b4da7613b5c56a05e079ea714c87cf4ab13f96` |
| 185000 | `94b261ebd60ca8b6120a33fc3cc6921e45a8cf70b15df3342f1929fe5b09ac12` |
| 277800 | `6673971417b6f90fe9d037eac94d3f8c20bfc8e8b7627321828de6211490d400` |

Configured reorg activation heights are 191,000 and 263,000, with maximum depth
10. The earlier `ConnectTip` expression is zero under its usual parent/tip
invariant. The substantive `CheckReorgDepth` check rejects rollback depth greater
than 10 after its activation, except during initial download; direct extensions
are accepted by that check. Minimum peers is configured as three but remains a
TODO. CLI option declarations do not wire `-maxreorg`/`-minreorgpeers` to enforcement.

The existing recommendation of 15 confirmations is not guaranteed finality.
Already-running nodes, fresh synchronization, and replay can make different
chain-selection decisions. Do not override these differences to make nodes agree
without preserving evidence and separately reviewing the consequences.

## Historical and test-network limits

`v3.0.0rc1` and `v3.0.0` schedule special issuance at 176,500; `v3.0` and
`v3.0.1` use 178,000; `v3.0.2`, `v3.1`, and `v3.2` use 180,000. The names are
distinct tags, not a reliable monotonic compatibility scale. Earlier releases
are not interchangeable replay baselines.

Testnet differs in activation heights, minimum-difficulty handling, and historical
assume-valid/minimum-work data. Added SOH fields are incompletely initialized
outside mainnet. Regtest selects LWMA at candidate height 200 even though its
no-retarget flag is set. See [known limitations](maintenance-notes.md) and the
[testing plan](compatibility-testing.md); do not extrapolate mainnet rules to
test networks or treat a passing regtest as full historical compatibility evidence.
