# Building Stohn Coin Core

Start with the [build status and preservation notes](doc/maintenance-notes.md#build-and-test-status).
The platform guides contain inherited recipes; they have not been revalidated by
the documentation-only handover work.

- [Linux and Unix](doc/build-unix.md)
- [macOS](doc/build-osx.md)
- [Windows / MinGW cross-compilation](doc/build-windows.md)
- [Native Windows / Visual Studio limitations](build_msvc/README.md)
- [Dependency versions](doc/dependencies.md)
- [Dependency build system](depends/README.md)

Record the exact source commit, build options, dependency versions, and platform.
Build in a separate development checkout, and test against copies of data before
considering a production installation. Do not overwrite an existing wallet or
production executable while experimenting. Keep Berkeley DB 4.8 support in builds
that need compatibility with legacy distributed wallets.
