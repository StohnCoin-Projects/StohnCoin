# Operating and preserving a Stohn full node

This guide describes the source baseline recorded in the
[maintenance notes](maintenance-notes.md). It is documentation, not evidence of a
new deployment or a completed recovery test. Review known software issues before
choosing a binary; no version is certified secure by this guide.

## Identify an existing installation first

Record the binary checksum, reported version, source/build provenance if available,
service user, actual command line, configuration path, data path, and selected
chain. A shell's `stohncoind` may differ from the executable running as a service.
Do not publish credentials, full configuration files, wallet files, or private logs.

Use the existing service's user and `-datadir`/`-conf` arguments for CLI requests.
Do not start a second daemon on its data directory. Service defaults can differ
from interactive defaults; see [init documentation](init.md).

| Platform | Interactive default mainnet data directory |
| --- | --- |
| Linux | `$HOME/.stohn/` |
| macOS | `$HOME/Library/Application Support/Stohn/` |
| Windows | `%APPDATA%\Stohn\` |

The configuration filename is `stohn.conf`. Non-mainnet chain data goes into
`testnet3/`, `regtest/`, or `signet/` beneath the selected base data directory.
The CLI must use the same network selection and base directory as its daemon.

## Prepare a separate new node

Obtain an identified Stohn binary or use the [build instructions](../INSTALL.md).
Preserve its checksum and license notices. Do not replace a production executable
or reuse the only copy of a historical wallet during setup.

Create a new, private data directory owned by the intended service user. The
following is a **documentation example** of a mainnet archival node configuration;
review it before manually creating a configuration file in that new directory:

```ini
chain=main
server=1
disablewallet=1
listen=1
prune=0

[main]
port=37218
rpcport=32717
# addnode=verified-soh-peer.example:37218
```

The `.example` hostname is a placeholder, not a peer. Obtain verified peers from
independent operators. `disablewallet=1` is appropriate for infrastructure nodes;
it does not erase wallets and does not make a wallet-enabled recovery build
unnecessary. `prune=0` retains blocks; it cannot restore blocks already pruned.

RPC defaults to loopback with local cookie authentication. Keep it private and
restrict cookie/data-directory access. Do not add public `rpcbind`/`rpcallowip`
settings. For a remotely administered node, use an authenticated private channel.
Permit inbound P2P TCP 37218 only if operating a publicly reachable mainnet node;
RPC TCP 32717 is not a public peer service.

After replacing the placeholder path with the new absolute directory, a Linux
operator can start and inspect that new node with:

```sh
stohncoind -datadir=/absolute/path/to/soh-main -daemon
stohncoin-cli -datadir=/absolute/path/to/soh-main getblockchaininfo
```

For service operation, inspect the [checked-in service examples](../contrib/init/README.md)
and their paths before installing anything. Keep OS security updates, disk space,
bounded logs, restart supervision, and alert ownership in the operating procedure.

## Testnet and regtest

Use a different base directory and configuration for each network. A testnet
example, subject to the [known test-network limitations](maintenance-notes.md#build-and-test-status), is:

```ini
chain=test
server=1
disablewallet=1
listen=1
prune=0

[test]
port=47218
rpcport=47217
# addnode=verified-soh-test-peer.example:47218
```

The header is `[test]`, not `[testnet]`. With this configuration, select the same
new base directory in both daemon and CLI commands; test chain data is placed in
its `testnet3/` subdirectory. There are no active configured testnet seeds, and no
working public testnet is established by these instructions. Never supply mainnet
peers or production wallets to it.

Regtest selection is `chain=regtest` (or `-regtest`), with section `[regtest]`, P2P
19444 and RPC 18443. The current regtest implementation has known SOH adaptation
issues and is not a validated substitute for mainnet-history tests. Retained
signet parameters must not be assumed to describe a working SOH signet.

## Read-only health checks

For an existing node, use its actual configuration paths rather than the example
paths above. The following RPCs inspect state; they do not submit transactions:

```sh
stohncoin-cli getnetworkinfo
stohncoin-cli getblockchaininfo
stohncoin-cli getconnectioncount
stohncoin-cli getchaintips
stohncoin-cli getdeploymentinfo
stohncoin-cli getblockhash 0
stohncoin-cli getblockhash 277800
```

Confirm `chain=main` and compare genesis/checkpoint hashes with the
[consensus reference](consensus-reference.md). Agree on a common recent height
with independent operators and compare `getblockhash HEIGHT` there, then examine
chainwork and tip progression. Matching local block/header heights only shows
that the node has processed headers it knows. A peer count does not measure
operator independence. Investigate persistently low outbound connectivity.

`verificationprogress` uses stale upstream statistics and is misleading.
`initialblockdownload=false` is latched and does not prove continued synchronization.
Check elapsed wall time, timestamps, and repeated tip observations. Block timing
is variable; one slow block does not by itself prove a failure. Keep host time
accurate because timestamp checks depend on it.

`getpeerinfo` includes addresses. If sharing a summary, remove addresses and other
identifiers; retain direction, connection type, protocol/subversion, and common
block/header heights. Do not publish raw logs or cookie contents.

## Archival backups and recovery

1. Preserve identified binaries, full source history, license notices, build
   records, configuration with secrets stored separately, and an agreed chain tip.
2. During scheduled maintenance, stop gracefully with the matching CLI's `stop`
   command and wait for process exit. Copy the complete data directory only after
   shutdown, or use a documented application-consistent backup method. Raw copies
   of live database files are not a validated backup procedure.
3. Keep private backups separate from public block archives. Data directories can
   contain wallets, RPC credentials/cookies, network private keys, and peer records.
   Exclude those from anything published. Back up wallets using the
   [wallet recovery procedure](wallet-compatibility.md#backup-and-recovery).
4. Store manifests with checksums, format/software provenance, height, block hash,
   chainwork, date, encryption/access requirements, and a tested restore location.
   Keep multiple copies under independent ownership.
5. Restore to a new directory with identified compatible software. Compare hashes
   with independent nodes before treating it as the accepted history. A copied
   chainstate database is a trusted operational backup, not trustless validation.

If tips diverge, preserve both histories and logs. Investigate peers, versions,
checkpoints, timestamps, and local reorg state. Do not use `invalidateblock`,
`reconsiderblock`, checkpoint changes, or reindexing as an unexplained repair.
Reindex/replay belongs on a copy with retained original evidence and enough blocks.
Existing reorg rules can distinguish a fresh replay from an already synchronized
node; consult the [testing plan](compatibility-testing.md).

## Minimal ongoing service

Aim for at least two independently operated archival nodes, private RPC, verified
peer contacts, and independently stored recovery archives. Size storage from actual
blocks, chainstate, indexes, logs, and growth; RPC `size_on_disk` covers block/undo
files, not total disk use. Do not prune the last complete history.

Assign someone to monitor block progress, peer diversity, divergence, disk/log
growth, backups, domain/certificate renewals, and applicable security advisories.
Nodes preserve and validate history; miners produce and secure new blocks. If
mining stops, new transactions cannot reliably confirm. No automatic, unchanged-rule
restart after indefinite inactivity is guaranteed.
