# Service configuration for stohncoind

Use the [node operation guide](node-operation.md) for network selection, private
RPC, read-only health checks, archival backups, and recovery. This page identifies
the checked-in service examples; no service installation was performed by the
documentation handover.

## Sample files

| Init system | Checked-in example |
| --- | --- |
| systemd | [stohncoind.service](../contrib/init/stohncoind.service) |
| OpenRC | [stohncoind.openrc](../contrib/init/stohncoind.openrc), [stohncoind.openrcconf](../contrib/init/stohncoind.openrcconf) |
| Upstart | [stohncoind.conf](../contrib/init/stohncoind.conf) |
| Legacy CentOS/SysV | [stohncoind.init](../contrib/init/stohncoind.init) |

These inherited examples need review for the target OS and installed binary.
Older documentation mentioned `org.stohn.stohncoind.plist`; that file is not
present in `contrib/init` in the assessed tree. No working macOS launch-agent
installation is established by that reference.

## Service user and paths

The systemd, OpenRC, and Upstart examples use `stohncoin` as the default user and
group, with binary `/usr/bin/stohncoind`, configuration
`/etc/stohncoin/stohn.conf`, and data directory `/var/lib/stohncoind`.
OpenRC also checks for a legacy `/var/lib/stohncoin/.stohncoin` directory.
Read the selected script and deployment overrides rather than assuming these
defaults describe an existing installation.

Systemd uses `/run/stohncoind/stohncoind.pid` and declares runtime, configuration,
and state directories with mode 0710. Restrict file permissions as well: directory
mode alone does not protect a world-readable wallet or RPC credential.

The legacy CentOS script has different behavior: its executable override is
`BITCOIND_BIN`, its argument override is `BITCOIND_OPTS`, and its default arguments
include `-disablewallet`. Review `DAEMONOPTS` and
`/etc/sysconfig/stohncoind` before use. These retained variable names are not
branding errors and are not renamed by documentation maintenance.

## Configuration and authentication

Command-line `-datadir` and `-conf` arguments in a service take precedence over
configuration-file settings. An interactive CLI using its own default directory
may contact the wrong node or fail to find the cookie.

Use the same user, base data directory, configuration path, and network selection
as the daemon when inspecting it with `stohncoin-cli`. RPC defaults to loopback
and, without a configured password, a per-start session cookie. Read access to
the cookie grants RPC access; protect it and keep RPC private. Use
[stohn.conf](stohn-conf.md) for syntax, not the stale port comments in the generated
example file.

## Installing or changing a service

For a separately authorized installation, identify and back up the current service
definition and data first, review the example's user/paths/permissions and
shutdown notifications, and use the target OS's supported service-management
procedure. Verify the exact installed executable, effective arguments, local RPC,
P2P reachability, clean shutdown, and restart behavior on a separate node.

Do not copy an example over an existing production service without reviewing
overrides and recovery. The systemd example contains an inherited description typo
and a documentation URL using `master` rather than `v26.x`; the service file itself
is unchanged by this documentation work.
