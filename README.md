Stohn Coin Core integration/staging tree
=====================================

https://stohncoin.org

What is Stohn Coin Core?
---------------------

Stohn Coin Core connects to the Stohn Coin peer-to-peer network to download and fully
validate blocks and transactions. It also includes a wallet and graphical user
interface, which can be optionally built.

License
-------

Stohn Coin Core is released under the terms of the MIT license. See [COPYING](COPYING) for more
information or see https://opensource.org/licenses/MIT.

Ports
=====================================

Stohn Core by default uses port `37218` for peer-to-peer communication that
is needed to synchronize the "mainnet" blockchain and stay informed of new
transactions and blocks. Additionally, a JSONRPC port can be opened, which
defaults to port `32717` for mainnet nodes. It is strongly recommended to not
expose RPC ports to the public internet.

| Function | mainnet | testnet |
| :------- | ------: | ------: |
| RPC     |   32717 |   47217 |
| P2P     |   37218 |   47218 |
