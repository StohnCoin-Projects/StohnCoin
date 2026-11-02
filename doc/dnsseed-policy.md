# Expectations for SOH DNS-seed operators

These expectations adapt the inherited Bitcoin Core discovery guidance for a
future SOH service. See the [SOH seeder guide](dns-seeder.md) for software validation,
DNS operation, and ownership requirements. No mainnet DNS-seed hostname is
configured in the assessed Core source, and this document deploys no service.

1. Secure the hosting, domain, DNS delegation, and access recovery. Publish the
   operator and backup contact, renewal responsibility, and service retirement
   procedure. Document any ownership transition and notify affected SOH operators;
   do not silently sell or transfer an established seed's trust identity.
2. Return functioning, fairly selected SOH peers. Check the network and service
   capabilities, expire stale results, and verify chain agreement independently.
   A reported software version alone is insufficient. Never substitute Bitcoin
   peer lists or imply that DNS answers establish blockchain validity.
3. Randomize a diverse eligible set. Do not target particular clients with
   different answers except for a disclosed operational necessity. Record
   filtering criteria and avoid dependence on a single operator/provider.
4. Use a documented TTL of at least one minute unless a reviewed operational reason
   requires otherwise. Limit DNS query logging and retention to what operation
   and incident response require; keep query records private.
5. Publish operating practices and a reachable contact. Crawler-derived public
   peer data may support independent discovery, but do not mix in private query
   data or unpublished operator details without permission.
6. If a service is compromised or cannot meet its commitments, notify SOH operators
   through available project channels and arrange a documented withdrawal or
   transition. Independent successors may establish new services under applicable
   licenses without permission from the original developers.

[Bitcoin seeder](https://github.com/sipa/bitcoin-seeder) is an upstream reference,
not a validated SOH implementation. Bitcoin project contacts are not SOH service
operators.
