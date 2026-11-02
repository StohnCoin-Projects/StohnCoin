Stohn Coin Core documentation
============================

Start with the [maintenance notes](maintenance-notes.md),
[installation entry point](../INSTALL.md), and
[documentation handover plan](documentation-plan.md). These distinguish verified
source settings from inherited recipes and work that still needs validation.

Handover guides
--------------

- [Full-node operation, backups, and hibernation](node-operation.md)
- [Peer discovery and DNS-seeder handover](dns-seeder.md)
- [Existing consensus and network reference](consensus-reference.md)
- [Wallet recovery and native mobile integration](wallet-compatibility.md)
- [Community maintainership and future releases](maintainer-transition.md)
- [Compatibility testing plan (not executed)](compatibility-testing.md)

These guides describe existing behavior and future procedures. They do not deploy
services, change software, or require a blockchain fork.

Setup
---------------------
Stohn Coin Core validates the SOH blockchain and, unless pruning is enabled,
stores its history. Measure the selected node's block, chainstate, index, and
wallet storage rather than using inherited Bitcoin disk-space estimates.
Synchronization time depends on available peers, hardware, and chain data.

To download StohnCoin, visit [stohncoin.org](https://stohncoin.org/).

Running
---------------------
The following are some helpful notes on how to run StohnCoin on your native platform.

### Unix

Unpack the files into a directory and run:

- `bin/stohncoin-qt` (GUI) or
- `bin/stohncoind` (headless)

### Windows

Unpack the files into a directory, and then run stohncoin-qt.exe.

### macOS

The current build names the bundle `StohnCoin-Qt.app`. For an identified release,
place its application bundle in your Applications folder and open it. Confirm
the release's actual filename and preserve an existing installation before replacement.

Building
---------------------
The following are developer notes on how to build StohnCoin on your native platform. They are not complete guides, but include notes on the necessary libraries, compile flags, etc.

- [Dependencies](dependencies.md)
- [macOS Build Notes](build-osx.md)
- [Unix Build Notes](build-unix.md)
- [Windows Build Notes](build-windows.md)
- [FreeBSD Build Notes](build-freebsd.md)
- [OpenBSD Build Notes](build-openbsd.md)
- [NetBSD Build Notes](build-netbsd.md)
- [Android Build Notes](build-android.md)

Development
---------------------
See [CONTRIBUTING.md](../CONTRIBUTING.md) for Stohn-specific contributions and
[test limitations](maintenance-notes.md#build-and-test-status) before relying on
the inherited test suite. Many detailed documents retain upstream Bitcoin
terminology or examples; they are not all validated SOH operating procedures.

- [Developer Notes](developer-notes.md)
- [Productivity Notes](productivity.md)
- [Release Process (inherited reference; see status note)](release-process.md)
- [Bitcoin Core Upstream Source Documentation (not SOH-specific)](https://doxygen.bitcoincore.org/)
- [Translation Process](translation_process.md)
- [Translation Strings Policy](translation_strings_policy.md)
- [JSON-RPC Interface](JSON-RPC-interface.md)
- [Unauthenticated REST Interface](REST-interface.md)
- [Shared Libraries](shared-libraries.md)
- [BIPS](bips.md)
- [SOH DNS-seed Operator Expectations](dnsseed-policy.md)
- [Benchmarking](benchmarking.md)
- [Internal Design Docs](design/)

### Miscellaneous
- [Assets Attribution](assets-attribution.md)
- [stohn.conf Configuration File](stohn-conf.md)
- [CJDNS Support](cjdns.md)
- [Files](files.md)
- [Fuzz-testing](fuzzing.md)
- [I2P Support](i2p.md)
- [Init Scripts (systemd/upstart/openrc)](init.md)
- [Managing Wallets](managing-wallets.md)
- [Multisig Tutorial](multisig-tutorial.md)
- [P2P bad ports definition and list](p2p-bad-ports.md)
- [PSBT support](psbt.md)
- [Reduce Memory](reduce-memory.md)
- [Reduce Traffic](reduce-traffic.md)
- [Tor Support](tor.md)
- [Transaction Relay Policy](policy/README.md)
- [ZMQ](zmq.md)

License
---------------------
Distributed under the [MIT software license](/COPYING).
