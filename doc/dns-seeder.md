# SOH peer discovery and DNS-seeder handover

This is an operating checklist for a future SOH seeder. The assessed repository
contains fixed-peer list tools, not a seeder daemon, and no validated external
SOH seeder build or command line has been identified. Documentation edits do not
deploy a service, change compiled seeds, or create a blockchain fork.

## Existing discovery

The mainnet source loads nine fixed IPv4 peers from
[`chainparamsseeds.h`](../src/chainparamsseeds.h) and configures no DNS-seed
hostnames. Testnet has no active configured seeds. See the dated observations in
the [maintenance notes](maintenance-notes.md#network-and-synchronization).

An existing node may have learned peers in `peers.dat` even if a fresh installation
cannot discover any. Preserve a private peer inventory with operator permission,
last verification time, network, protocol, services, and common-chain checks.
Use multiple independent operators/providers rather than treating many IPs as
independent ownership. A sanitized public roster can help future operators.

An operator can explicitly configure verified SOH peers with `addnode=host:37218`
in `[main]`, or port 47218 in `[test]`. `addnode` retains automatic discovery;
`connect` restricts automatic connections and should not be copied as an equivalent.
These are future manual configuration choices, not changes made by this guide.

## Select and validate seeder software

[Bitcoin seeder](https://github.com/sipa/bitcoin-seeder/blob/master/README) is an
upstream reference, not a ready-to-run SOH binary. Select an identified version
and review its network settings and crawler behavior before any SOH deployment.
Its upstream build and DNS instructions are starting points for that separate
work; do not publish Bitcoin peer results under a SOH seed name.

A SOH adaptation must correctly handle:

- P2P magic `f74149c4`, mainnet port 37218, and SOH protocol negotiation.
- SOH genesis and independently agreed historical blocks. A version handshake
  alone cannot establish that a peer follows the accepted SOH chain; document
  whether the crawler checks headers or relies on independent validation.
- SOH agent names and service flags, without inheriting Bitcoin-specific height,
  version, difficulty, checkpoint, or seed bootstrap assumptions.
- IPv4/IPv6 address serialization, usable advertised ports, and any supported
  service-bit DNS queries. Advertise only capabilities actually observed.
- Timeouts, backoff, resource bounds, stale-entry expiry, and a rebuildable peer
  database. Reject wrong-network peers and avoid a fixed privileged peer set.

If adaptation requires code changes, that is outside the approved documentation
work. A future deployment record must identify its source, build recipe, tests,
binary checksum, startup arguments, service user, and data directory. No guessed
SOH launch command is provided in place of that evidence.

## DNS and service operation

1. Arrange registrar/DNS control and a durable hostname with named primary and
   backup operators. Use a dedicated seed subdomain and authoritative nameserver;
   document the required NS delegation and any glue records.
2. Confirm the chosen daemon's DNS behavior, listening address/port, and support
   for required UDP/TCP queries. Plan access to DNS port 53 through a reviewed
   service setup or DNS frontend; do not run the crawler as root by default.
3. Bootstrap the crawler with independently verified SOH peers. In a private test,
   confirm rejection of Bitcoin/wrong-chain peers and correct expiry of stale
   results before public delegation. Check responses against independent nodes.
4. After a separately authorized deployment, verify authoritative answers from an
   external resolver and verify that returned hosts actually serve SOH P2P.
   Monitor answer quality, uptime, traffic/resource limits, and domain renewal.
5. Publish an operational contact and selection policy. Minimize query logging
   and retention. Return a fair, diverse sample; disclose significant filtering.
   Use a documented TTL of at least one minute unless a reviewed operational
   reason justifies otherwise. A seeder is peer discovery, not a chain authority.
6. Back up delegation, service configuration, build/provenance records, and access
   recovery instructions. Store credentials privately. The crawler database must
   be reconstructible from independent SOH peers if its owner disappears.

## Existing clients and succession

Creating a DNS service does not make current binaries query it: their mainnet
DNS-seed list is empty. Existing users need explicit verified peer/seed
configuration. A compiled discovery update would be a separate software proposal,
not part of this handover. No consensus change is necessary to document or
explicitly configure an already compatible peer.

Record domain owner, delegation owner, hosting account, renewal dates, access
recovery, backup operator, and withdrawal procedure. If a seed is retired or
compromised, notify SOH operators through available project channels and preserve
independent discovery alternatives. Future teams can establish new domains under
the applicable software licenses without original-developer approval.
