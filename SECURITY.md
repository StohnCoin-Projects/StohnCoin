# Security policy

## Support status

Stohn Coin Core is maintained separately from Bitcoin Core. Bitcoin Core's support
schedule does not establish security support for SOH, and the presence of a version
or release tag does not establish that upstream fixes have been backported.

This repository does not currently document a verified Stohn-specific security
support schedule or a dedicated private reporting address and encryption key.
Future maintainers should publish these details when they can support them.
See the [maintenance notes](doc/maintenance-notes.md) for findings from the initial
source assessment. Documentation updates do not fix the recorded software issues.

## Reporting a vulnerability

Check the repository's [Security page](https://github.com/StohnCoin-Projects/StohnCoin/security)
for an available private reporting option. If private reporting is unavailable,
open a [GitHub issue](https://github.com/StohnCoin-Projects/StohnCoin/issues)
requesting a private contact, without publishing vulnerability details, exploit
instructions, private keys, credentials, or wallet files.

Bitcoin Core security contacts and signing keys are not Stohn Coin support
contacts. For an issue independently affecting upstream Bitcoin Core, follow
[upstream's security policy](https://github.com/bitcoin/bitcoin/blob/master/SECURITY.md)
as well; do not assume that reporting upstream also notifies SOH maintainers.

## Information to prepare privately

Record the affected Stohn version, binary checksum and source commit if known,
operating system, relevant build options, and reproduction steps using synthetic
data. Sanitize logs before sharing. Maintain security fixes as separate reviewed
changes with compatibility evidence; do not mix them into documentation cleanup.
