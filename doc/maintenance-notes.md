# Maintenance notes and known limitations

These notes record a source assessment of Stohn Coin Core at commit
`87cb7eed35560c894a6129c93e2e2853e454c8c9`, dated October 31, 2026.
They describe existing behavior and unresolved work. Documentation changes do
not fix these issues or change blockchain rules.

The November 1, 2026 documentation handover adds guides for
[node operation](node-operation.md), [peer discovery](dns-seeder.md),
[existing consensus](consensus-reference.md), [wallet recovery](wallet-compatibility.md),
[maintainer succession](maintainer-transition.md), and
[future compatibility testing](compatibility-testing.md). Those procedures have
not been executed by the documentation work and do not close the gaps below.

## Source and running-version evidence

The assessed branch is `v26.x`, with software version `3.3.0` and protocol `70020`.
Its identified Bitcoin Core 26.x baseline is `04edf9f58`; subsequent SOH changes
include Scrypt PoW, difficulty transitions, special issuance, network identifiers,
wallet defaults, timestamp handling, and reorganization restrictions.

At assessment time, the newest published release was `v3.2`, while the branch
contained later changes. An operator supplied mainnet RPC output showing
`/StohnCore:3.3.0/`, protocol `70020`, height 438,611, matching known header height,
and `initialblockdownload=false`. This identifies that node's reported version,
not its exact source commit or independent network-wide agreement. Binary hashes,
build provenance, and comparison with other operators remain outstanding.

## Build and test status

Platform guides are inherited and partly adapted recipes, not freshly verified
build results. Record the source commit, OS/toolchain, dependencies, and options
when testing them in an isolated checkout.

- **Legacy wallets:** preserve Berkeley DB 4.8 compatibility. A build using
  `--with-incompatible-bdb` is unsuitable as a compatibility-preserving default.
  Automatic backend detection can omit legacy support or the entire wallet
  without a configuration error. Verify the final configure summary before recovery.
- **Native Windows:** the MSVC generator still searches for `libbitcoin_*` source
  groups renamed to `libstohncoin_*` in Automake. Read-only parser inspection
  found missing source groups. The generator is not repaired by this work.
- **Tests:** inherited subsidy tests assume 50-coin rewards; some difficulty
  tests pass a zero target timespan. Python helpers retain Bitcoin-style PoW
  solving and maturity 100, while SOH uses Scrypt and maturity 20.
- **Test networks:** added SOH consensus fields are not initialized consistently
  outside mainnet. Regtest also selects LWMA before the no-retarget shortcut at
  its fork boundary. Do not treat these networks as a validated SOH test harness.
- **Completed assessment checks:** 325 Python files parsed without syntax errors;
  the three configured genesis header hashes/Scrypt proofs and five Scrypt test
  fixtures were independently reproduced. These were not C++ test-suite runs.

No full platform compilation, release-signature verification, chain replay,
sanitizer run, or historical wallet restoration was completed in the initial
assessment. See the [test instructions](../test/README.md) for inherited commands,
subject to these limitations.

## Network and synchronization

Mainnet has nine compiled IPv4 seeds and no configured DNS seeds in the assessed
source. One check from the audit environment reached none of those addresses;
this does not establish network-wide unavailability. The operator's node reported
27 connections, including one outbound connection. Fresh-node discovery and
independent peer availability need separate verification.

The advertised public RPC endpoint failed TLS hostname validation during the
assessment. Do not rely on that observation as a permanent service status or
disable TLS verification to work around it. Verify service identity and current
availability before documenting a production dependency.

`verificationprogress` uses inherited Bitcoin transaction counts/rates in
[`chainTxData`](../src/kernel/chainparams.cpp). It can report a tiny fraction on a
node that has processed all known headers. `initialblockdownload` latches to false
after its checks pass; neither field alone establishes current mainnet agreement.
Compare recent block hashes and chainwork with independent nodes.

The checked-in [example configuration](../share/examples/stohn.conf) has stale
port descriptions. Use the [README port table](../README.md#network-ports) and
the selected binary's help output. The configuration-generation script also
defaults to the old executable name; see [stohn.conf documentation](stohn-conf.md).

## Consensus and wallet boundaries

The implementation remains authoritative: see
[`chainparams.cpp`](../src/kernel/chainparams.cpp), [`pow.cpp`](../src/pow.cpp),
[`validation.cpp`](../src/validation.cpp), and [`chain.h`](../src/chain.h).

- Reorganization behavior depends on local state, including an initial-download
  exception. The minimum-peer check is unfinished. The declared `-maxreorg` and
  `-minreorgpeers` options are not wired to enforcement in the assessed code.
  Do not describe 15 confirmations as guaranteed finality.
- `MAX_MONEY` remains a consensus amount-range limit of 21,000,000 SOH, not a
  statement of total scheduled issuance. Do not change it to match a supply claim.
- Early public tags schedule special issuance at different heights. They are
  not interchangeable historical-validation baselines.
- Tags through `v3.0.1` use P2SH prefix 5; `v3.0.2` and later use 63. Historical
  recovery must retain redeem scripts and explicitly identify the SOH context.
- Legacy HD wallets and descriptor wallets use different derivation schemes.
  Descriptor generation currently uses mainnet coin type `0'`. A Core backup
  is not automatically recoverable from a BIP39 mobile mnemonic.
- The signed-message prefix remains `Bitcoin Signed Message:\n`. Changing it
  as branding cleanup would change interoperability.
- Taproot encoding exists, but the mainnet minimum activation height is 709,632.
  Encoding support does not establish activation. Verify deployment state before
  offering Taproot receiving or spending.
- Current-height timestamp buffers are shorter than the historical allowance.
  Compare birthday-based recovery with a full historical scan before concluding
  that a wallet has no funds.

## Security work remains separate

The assessment found source-level matches to the upstream
[script-validation lifetime issue](https://bitcoincore.org/en/2026/05/05/disclose-cve-2024-52911/)
and [non-Linux socket backpressure issue](https://bitcoincore.org/en/2026/10/28/disclose-tcp-oom/).
Other upstream networking, resource-exhaustion, and storage advisories also require
SOH applicability review. A source-level match is not an exploit demonstration
against a deployed binary, and this list is not an exhaustive security audit.

Dependency pins also need review against the actual build and component usage.
Do not assume Bitcoin Core updates apply automatically to SOH or merge a newer
upstream release as routine cleanup. Any software fix needs its own scope,
review, and compatibility evidence. See [SECURITY.md](../SECURITY.md) for reporting.
