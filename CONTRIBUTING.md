# Contributing to Stohn Coin Core

Contributions, review, testing, and documentation are welcome at
[StohnCoin-Projects/StohnCoin](https://github.com/StohnCoin-Projects/StohnCoin).
Submit SOH node, wallet, GUI, and documentation proposals to this repository.
Bitcoin Core's repositories, mailing lists, and issue trackers are upstream
resources, not the destination for Stohn-specific support or contributions.

This guide is adapted from the inherited Bitcoin Core contributor guidance.
Preserve its attribution and all applicable licenses when reusing project material.

## Current documentation handover scope

The [documentation handover plan](doc/documentation-plan.md) covers documentation only.
It changes no executable behavior and requires no hard fork or soft fork.

Keep the following outside that work:

- Source code, scripts, build definitions, CI workflows, dependencies, and generated files.
- Genesis, consensus rules, difficulty, rewards, checkpoints, and chain-selection behavior.
- Address formats, key derivation, transaction serialization, signed-message prefixes,
  wallet formats, data-directory defaults, and configuration defaults.
- Production nodes, deployments, existing releases and tags, and historical Git data.

Correct misleading prose and command examples to describe the existing software.
Preserve Bitcoin references that are attribution, technical standards, historical
records, library/build identifiers, or existing protocol behavior. Do not perform
a repository-wide word replacement.

## Proposing a change

1. Check existing [issues](https://github.com/StohnCoin-Projects/StohnCoin/issues)
   and [pull requests](https://github.com/StohnCoin-Projects/StohnCoin/pulls).
2. Create a topic branch or an independent source copy. A GitHub repository fork
   is a copy of source code; it does not itself create a new blockchain.
3. Keep the proposal focused. Separate documentation, build repairs, security
   fixes, and behavior changes so each can receive appropriate review.
4. Submit a pull request describing the problem, final changes, compatibility
   impact, validation performed, and remaining uncertainties.

Use a descriptive prefix such as `doc:`, `build:`, `test:`, or `wallet:`. Use
`doc:` for this handover work. Do not mix formatting or renaming with functional
changes. Reviewers should identify the exact commit they reviewed and distinguish
source inspection from tests they actually ran.

## Validation and compatibility

For documentation changes, check links, command names, paths, and stated defaults
against this repository. Clearly identify historical or unverified recipes.
Compilation or production changes are not needed to validate prose-only edits.

For separately proposed software changes, record the exact baseline and test
results. Follow the [developer notes](doc/developer-notes.md) and review the
[test limitations](doc/maintenance-notes.md#build-and-test-status) before treating
the inherited suite as evidence of SOH compatibility. Use isolated data and wallet
copies. Never include private keys, wallet backups, or credentials in a PR.

Consensus and wallet compatibility require special review even for apparently
small refactors. Do not blindly merge a newer Bitcoin Core release or silently
change validation to make a test pass. Such proposals are outside the current
documentation scope. Report vulnerabilities using [SECURITY.md](SECURITY.md).

## Independent development and maintainership

Anyone may develop or distribute an independent version under the applicable
licenses without permission from the original developers. The MIT license's
notice requirements and individual component licenses still apply. Access to
the existing GitHub organization, domains, signing identities, or hosting accounts
requires separate ownership arrangements; permission to use the source does not
convey those accounts or identities.

Maintainership of this repository concerns review, merging, and publication. It
does not give maintainers authority to change the rules followed by independently
operated nodes. Preserve existing releases, tags, and history; publish future
releases under new version identifiers with documented provenance.

Use the [maintainership and release guide](doc/maintainer-transition.md) for Stohn
succession and future release review. The [release-process document](doc/release-process.md)
retains inherited implementation details and is not a standalone SOH checklist.

## Copyright and licensing

Contributions use the [MIT license](COPYING) unless a file or component specifies
otherwise. When contributing work by others, preserve the original authors,
license notices, and source attribution. Do not remove Bitcoin Core copyright
notices when correcting project branding.
