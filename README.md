Stohn Coin Core
===============

Stohn Coin Core connects to the Stohn Coin (SOH) peer-to-peer network to download
and validate blocks and transactions. The source also includes an optional wallet
and graphical interface.

- Project website: https://stohncoin.org/
- Source and contributions: https://github.com/StohnCoin-Projects/StohnCoin
- [Documentation and build instructions](doc/README.md)
- [Maintenance notes and known limitations](doc/maintenance-notes.md)
- [Documentation handover plan](doc/documentation-plan.md)
- [Security policy](SECURITY.md)

For a community handover, start with [node operation](doc/node-operation.md) and
[maintainer succession](doc/maintainer-transition.md). Technical references cover
[existing consensus](doc/consensus-reference.md),
[wallet compatibility](doc/wallet-compatibility.md), and the
[compatibility testing plan](doc/compatibility-testing.md).

Compatibility and preservation
------------------------------

This project derives from Bitcoin Core and contains Stohn-specific consensus and
wallet behavior. A newer Bitcoin Core release is not a substitute for Stohn Core.
Preserve existing blockchain history, wallet backups, release artifacts, and Git
history when maintaining the project.

The documentation handover work does not change executable code or network
rules. Documentation-only changes require no hard fork, soft fork, node upgrade,
or wallet migration. See [contributing](CONTRIBUTING.md) for the scope of this work.

Block confirmation recommendation
---------------------------------

The existing project recommendation is to wait for at least 15 block
confirmations. This is a risk guideline, not a guarantee of finality or protection
against an attack. Wallet developers should review the existing reorganization
behavior described in the [maintenance notes](doc/maintenance-notes.md).

Network ports
-------------

These are the defaults in the audited source; command-line or configuration
settings can override them. Keep RPC access private.

| Function | Mainnet | Testnet | Regtest |
| :------- | ------: | ------: | ------: |
| RPC | 32717 | 47217 | 18443 |
| P2P | 37218 | 47218 | 19444 |

The configuration file is `stohn.conf`. See the [configuration reference](doc/stohn-conf.md)
for data-directory locations and network sections.

License and attribution
-----------------------

Stohn Coin Core is distributed under the [MIT license](COPYING). Preserve Bitcoin
Core attribution and the notices included with individual third-party components;
some components have their own license terms. Bitcoin references that describe
upstream history, standards, libraries, or protocol behavior are intentional.
