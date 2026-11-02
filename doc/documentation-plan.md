# Documentation handover plan

## Objective and scope

Prepare the existing Stohn Coin Core repository for a future community maintainer
with the minimum documentation changes needed to understand, build, and operate
the existing software. All three phases are documentation-only: no hard fork, soft
fork, node upgrade, or wallet migration is required by these edits.

The source assessment used commit
`87cb7eed35560c894a6129c93e2e2853e454c8c9` on `v26.x`. Record this baseline
when interpreting the [maintenance notes](maintenance-notes.md).

## Phase 1: correct the entry points

Status: complete. At phase completion, the documentation was local and uncommitted.

- [x] Update the root README, installation entry point, and documentation index.
- [x] Correct Stohn project routing in contributor and security guidance.
- [x] Correct platform build examples without editing the build system.
- [x] Document the initial build, test, and compatibility limitations.
- [x] Mark inherited release and seed procedures that must not be followed unchanged.
- [x] Check links, source-backed defaults, whitespace, and the documentation-only diff.

Deliverable: a reviewable local documentation diff. This phase does not compile,
install, publish, or deploy software, and does not certify current OS support or
the security of an existing binary.

## Phase 2: document operation and succession

Status: complete on November 1, 2026. At phase completion, no commit,
publication, deployment, or software change had been made.

- [x] Document mainnet and testnet configuration, read-only health checks, peer
  discovery, archival backups, and recovery without publishing credentials.
- [x] Describe how a future operator would run a SOH-specific DNS seeder, including
  the absence of configured mainnet DNS seeds and the need to adapt and validate
  external seeder software. No seeder is deployed as part of this work.
- [x] Record existing consensus parameters and historical boundaries as a compatibility
  reference, with links to the implementation rather than replacement rules.
- [x] Document address/key formats, Core wallet recovery, and mobile-wallet integration
  considerations. Do not implement a wallet, backend, or new derivation convention.
- [x] Write a maintainership transition and release guide covering source/binary
  provenance, licenses, signatures, ownership, independent development, and known
  validation gaps. Preserve existing releases and Git history.
- [x] Record a compatibility testing plan for historical chain data and wallet copies;
  executing builds or recovery tests is separate work.

## Phase 3: final documentation review

Status: complete on November 1, 2026. At phase completion, no commit,
publication, deployment, or software change had been made.

- [x] Review the combined documentation diff for technical accuracy and scope.
- [x] Check local links, heading targets, examples, and source-backed parameters.
- [x] Correct misleading wording without changing executable behavior.
- [x] Confirm source, logging, configuration, dependencies, licenses, and Git refs
  remain unchanged; leave all documentation local and uncommitted.

No log cleanup, software fix, build, commit, push, or deployment was part of this phase.

On November 2, 2026, the repository owner separately authorized committing and
pushing the reviewed documentation. That authorization does not include a software
release, node upgrade, or deployment. The validation records below describe the
state at each phase's completion, before publication.

## Boundaries for all phases

Edit human-readable documentation only. Leave source code, build scripts, CI,
dependencies, executable scripts, generated files, and configuration files unchanged.
Code blocks inside documentation may be corrected to show existing commands;
no commands affecting production are executed by this work.

Do not change consensus, checkpoints, seeds, protocol strings, wallet formats,
addresses, defaults, or chain statistics in source. Known defects remain recorded
for a separate proposal; no phase includes a software fix or deployment.

Preserve license/attribution files, historical release notes, releases, tags,
and all Git history. Correct project-name references selectively. Keep Bitcoin
references that identify upstream work, standards, libraries, build filenames,
and existing protocol behavior.

## Validation record

### Phase 1

Edited documentation:

- Root: `README.md`, `INSTALL.md`, `CONTRIBUTING.md`, `SECURITY.md`.
- Build/reference entry points: `doc/README.md`, `doc/build-unix.md`,
  `doc/build-osx.md`, `doc/build-windows.md`, `build_msvc/README.md`,
  `doc/dependencies.md`, `doc/stohn-conf.md`, `test/README.md`.
- Inherited procedure status: `doc/release-process.md`, `contrib/seeds/README.md`.
- New handover records: `doc/documentation-plan.md`, `doc/maintenance-notes.md`.

Validation at Phase 1 completion: the diff was limited to these 16 Markdown files.
New/changed local Markdown links were checked for existing targets and headings; source-backed port,
configuration, and compatibility references were inspected. `git diff --check`
passed. Source, build scripts, dependencies, configuration files, license files,
release artifacts, and production nodes were not modified. No binaries were built
or executed, and no runtime compatibility test is claimed for these prose edits.

### Phase 2

New guides:

- [Node operation, archival backups, and hibernation](node-operation.md).
- [SOH discovery and DNS-seeder handover](dns-seeder.md).
- [Existing consensus and network reference](consensus-reference.md).
- [Wallet recovery and native mobile integration](wallet-compatibility.md).
- [Community maintainership and future releases](maintainer-transition.md).
- [Compatibility testing plan](compatibility-testing.md).

Updated entry points: root `README.md` and `CONTRIBUTING.md`; `doc/README.md`,
`doc/release-process.md`, `doc/maintenance-notes.md`, and this plan; discovery
references `contrib/seeds/README.md` and `doc/dnsseed-policy.md`; service/file
references `doc/init.md`, `contrib/init/README.md`, and `doc/files.md`.

Validation: all new/changed local Markdown links and heading targets were checked.
Network magic, ports, genesis IDs, the six mainnet checkpoints, address parameters,
and relevant operational references were checked against source. Historical
issuance heights and P2SH prefixes were checked against all seven public tags.
Whitespace checks passed. The combined work from both phases is limited to 26
Markdown files; protected source, build, configuration, dependency, and license
files have no diff. Phase 1 documents outside the listed entry points are retained.

No binaries, nodes, services, wallet recovery, or compatibility tests were run by
the documentation work. The guides record planned procedures and unresolved
evidence, including the lack of a validated SOH seeder build; they do not certify
security or authorize any implementation, deployment, or consensus change.

### Phase 3

Reviewed the combined changes across all 26 Markdown files. Checked 245 local
links and heading targets, balanced fenced blocks, and syntax-checked eight
new or changed shell examples without executing them. Source-backed network,
address, genesis, checkpoint, and historical release parameters were rechecked.
`git diff --check` passed.

Corrected build guidance to explain that missing wallet dependencies can silently
disable wallet support, made the configuration-copy example preserve an existing
configuration, and corrected the macOS application bundle name. Updated the plan
and contributor guidance to reflect the completed three-phase documentation work.

The combined diff remains documentation-only. Source (including logging), build
scripts, configuration, dependencies, and license files are unchanged. HEAD and
Git refs were unchanged at review completion, and all work was local and
uncommitted. No binaries were built or executed, and no nodes, wallets, services,
or production systems were altered. Runtime compatibility tests and live infrastructure validation remain
separate work; this review does not certify the software's security.
