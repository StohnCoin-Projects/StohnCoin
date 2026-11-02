# Community maintainership, releases, and revival

This guide supports independent operation of the existing SOH chain. It changes
no software and does not declare an ownership transfer, a new release, or security
support. Begin with the [maintenance notes](maintenance-notes.md),
[node guide](node-operation.md), and [compatibility reference](consensus-reference.md).

## Independent development and attribution

The [MIT license](../COPYING) permits independent use, modification, and distribution
subject to its notice requirements. Original-developer approval is not required
for a source fork or independent development. Preserve Bitcoin Core and Stohn
attribution and all component-specific licenses. For example,
[`scrypt.cpp`](../src/crypto/scrypt.cpp) includes a BSD-style redistribution notice.
Redistributed binaries also need the applicable component notices.

Using the source does not transfer existing organizations, domains, signing
identities, accounts, reserves, or infrastructure. A successor can establish new
repositories, domains, contacts, and signing keys under the applicable licenses,
clearly identifying the operator and source provenance without claiming control
of the original accounts. Do not add a requirement for original-team approval.

## Preservation package

Arrange at least two independently stored copies, with access and restoration
instructions usable without the current team's machines. Preserve:

- Complete reachable Git history, branches, and tags, including original commit
  identifiers and signatures. Verify the archive can be cloned/restored and its
  objects checked. A source ZIP is not a substitute for Git history.
- Historical source archives, binary releases, checksums, detached signatures,
  attestations, license files, and the identities used to verify them. Never move
  old tags, replace old release assets, or rewrite history to tidy the project.
- Compiler/toolchain information, dependency sources and hashes, build options,
  packaging recipes, and signing/notarization records. External downloads can
  disappear; record which inputs are missing or cannot legally be redistributed.
- Complete archival block data and private operational backups with dated hash,
  height, and chainwork manifests, plus a documented restore/replay procedure.
- A public issue/limitations record and a private operational inventory, separated
  from wallet keys, credentials, personal information, and sensitive reports.

Do not publish a raw data-directory archive or credential-bearing Git config.
Record whether a backup is a trusted chainstate copy or blocks for independent
validation. No mainnet assumeUTXO snapshot is configured by this baseline.

## Ownership and service register

The initial audit did not verify account owners, renewal dates, funding, or transfer
access. Complete these fields with the current operators; do not infer ownership
from a domain name, commit author, or server address. Keep secrets in a private
credential store, never this repository.

| Asset to inventory | Record for the successor |
| --- | --- |
| `StohnCoin-Projects/StohnCoin` organization/repository | Admins, recovery access, mirrors, branch protection, issue/security contacts |
| `guix.sigs.stohn` and `stohn-detached-sigs` | Mirrors, signer fingerprints, provenance, verification instructions |
| `stohncoin.org` and its RPC/possible seed/Electrum names | Registrar, DNS owner, hosting, renewal, account recovery, backup operator |
| `stohnexplorer.com` | Registrar, hosting, application source, database rebuild, independent node dependency |
| Full nodes, peer hosts, pools, miners | Operators, providers, funding, P2P reachability, monitoring and exit procedures |
| TLS and release signing | Renewal, replacement identities, verification records, expiry/revocation procedures |
| Windows/macOS distribution | Signing/notarization account control, successor certificates, release verification |
| Future mobile support | Stack upstream relationship or app-store ownership, backend operation, recovery documentation |
| Bridge/WSOH systems | Separate custody, reserves, redemption, administration, and ownership arrangements |
| Community/support channels | Admins, recovery access, security contacts, announcement alternatives |

Identify the primary and backup responsible person, service purpose, dependency,
ongoing cost, renewal date, recovery method, and retirement plan for each entry.
Transfer account access through the provider's supported process. Use new
successor signing keys rather than casually copying old private signing keys.
Publish fingerprints and trust-transition evidence through independent channels;
if the original signer is unavailable, document the new trust basis honestly.

## Taking over node operation

1. Establish public maintainer/security contacts and a private access register.
   Preserve originals before changing accounts or services.
2. Identify the exact deployed binaries and source/build evidence. The operator's
   observed version 3.3.0 alone does not identify its source commit.
3. Restore independent archival nodes from copies. Compare genesis, existing
   checkpoints, agreed recent hashes, and chainwork with independent operators.
   Retain conflicting histories if agreement is not yet established.
4. Confirm peer discovery, current miners, backup restoration, monitoring, domain
   renewals, and ability to replace each single-owner service. A new seeder needs
   the separate validation described in the [discovery guide](dns-seeder.md).
5. Publish supported versions and known limitations only to the extent the new
   team can sustain them. Triage security reports independently of documentation
   maintenance. If essential validation remains missing, state it publicly.

## Procedure for a future release

This is a review sequence, not authorization to publish software. Documentation
changes alone require no node upgrade, hard fork, or soft fork. The detailed
[inherited release process](release-process.md) remains an implementation reference;
its Bitcoin project destinations and periodic chain-parameter updates must not
be followed as an automatic SOH checklist.

1. Define the final change and exact baseline commit. Separate documentation,
   security fixes, build repairs, and any behavior changes. Preserve consensus,
   wallet formats, address formats, defaults, and accepted history for a compatible
   maintenance release; do not bundle upstream modernization or discovery changes.
2. Open a reviewable proposal in the successor's identified repository. Record
   what the change does, compatibility risks, test evidence, unresolved failures,
   and reviewer identities/commits. Community members may review and contribute
   without original-team permission. Merging a PR does not control others' nodes.
3. Reproduce builds in isolated environments and apply the
   [compatibility plan](compatibility-testing.md). Resolve or explicitly block on
   unexplained chain, serialization, signature, or recovery mismatches. Do not
   claim every test passed when known failures remain.
4. For a software release, choose a new version/tag and retain existing identifiers.
   Record source commit, dependency revisions/checksums, toolchains, platforms,
   build flags, release notes, known limitations, and reproducible-build results.
   A binary checksum fingerprints a file; source mapping also requires build or
   reproducibility evidence.
5. Have independent builders compare unsigned outputs. Keep unsigned reproducible
   artifacts distinct from platform-signed/notarized artifacts. Document all
   transformations, including historical manual macOS packaging steps. Preserve
   the actual final artifact checksums and verify applicable licenses/notices.
6. Use documented successor keys to sign the tag/manifests and publish verifier
   fingerprints, checksums, detached signatures, and attestations. Verify signatures
   against independently established identities, including historical expiry and
   revocation status. Do not silently replace an old signature or identity.
7. After release review, publish new artifacts and an explicit operator procedure
   covering backups, clean shutdown, wallet compatibility, and limitations. Test
   on separate copies before any production rollout. A wallet/database opened by
   a newer build may need its pre-upgrade backup for rollback; replacing a binary
   alone is not a complete rollback procedure.

If a proposal cannot preserve the accepted rules/history, it is outside the
compatible maintenance scope. Do not describe it as documentation cleanup or
assume approval to deploy it. No such proposal is implemented by these guides.

## Hibernation and later revival

Reduce optional services only after documenting dependencies, public commitments,
and replacement/recovery paths. Keep complete history, identified compatible
software, independently owned nodes/backups, and miners if continued transactions
are expected. Assign funded responsibility for advisories, disk/log limits,
restoration exercises, and domain/certificate expiry. Full nodes do not replace
mining and no fixed confirmation count guarantees finality.

Before resuming services after inactivity, verify archive integrity, keys and
certificates, binary provenance, peers, and independent chain agreement on copies.
Review difficulty and reorg behavior under unchanged rules; a mining restart may
not be immediate. Preserve the last accepted history if nodes disagree. Do not
create a replacement genesis, reset history, change difficulty, or bypass validation
under the label of revival. Bridges and wrapped-token obligations need their own
continuity plan even if native SOH wallets remain available.
