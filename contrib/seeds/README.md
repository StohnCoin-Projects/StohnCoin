# Stohn peer seed reference

The compiled peer addresses are in
[src/chainparamsseeds.h](../../src/chainparamsseeds.h). The assessed mainnet source
has nine fixed IPv4 entries and no configured DNS-seed hostnames; testnet has no
active configured seed list. See [maintenance notes](../../doc/maintenance-notes.md#network-and-synchronization)
for the dated connectivity observations and their limitations.

This directory contains inherited list-generation tools, not a DNS-seeder daemon.
The former instructions fetched Bitcoin's seed list from `bitcoin.sipa.be`.
That is upstream Bitcoin data, not a source of SOH peers. Do not regenerate Stohn's
compiled seeds from it. The current `makeseeds.py` filters also contain Bitcoin
agent/version and height assumptions that need separate review before SOH use.

Preserve the checked-in seed lists and generated header during documentation
maintenance. A future discovery update requires independently verified SOH peers
and review of network identity, service flags, availability, and operator diversity.
Editing this README does not update seeds or deploy a service. Operating a new DNS
seeder alone would not make existing binaries query it automatically.

Use the [SOH discovery and seeder guide](../../doc/dns-seeder.md) for the operating
and ownership checklist. It records the absence of a validated SOH seeder build;
any software adaptation, source change, or deployment is separate work.
