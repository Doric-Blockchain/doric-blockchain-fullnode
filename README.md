## Go Doric

Official Golang implementation of the Doric protocol.

Automated builds are available for stable releases and the unstable master branch. Binary
archives are published at https://github.com/Doric-Blockchain/doric-blockchain-fullnode/releases.

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions](https://geth.ethereum.org/docs/install-and-build/installing-geth).

Building `geth` requires both a Go (version 1.13 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
make geth
```

or, to build the full suite of utilities:

```shell
make all
```

## Executables

The go-ethereum project comes with several wrappers/executables found in the `cmd`
directory.

|    Command    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :-----------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  **`geth`**   | Our main Doric CLI client. It is the entry point into the Doric network (main-, test- or private net), capable of running as a full node (default), archive node (retaining all historical state) or a light node (retrieving data live). It can be used by other processes as a gateway into the Doric network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports. `geth --help` and the [CLI page](https://geth.ethereum.org/docs/interface/command-line-options) for command line options.          |
|   `abigen`    | Source code generator to convert Doric contract definitions into easy to use, compile-time type-safe Go packages. It operates on plain [Doric contract ABIs](https://docs.soliditylang.org/en/develop/abi-spec.html) with expanded functionality if the contract bytecode is also available. However, it also accepts Solidity source files, making development much more streamlined. Please see our [Native DApps](https://geth.ethereum.org/docs/dapp/native-bindings) page for details. |
|  `bootnode`   | Stripped down version of our Doric client implementation that only takes part in the network node discovery protocol, but does not run any of the higher level application protocols. It can be used as a lightweight bootstrap node to aid in finding peers in private networks.                                                                                                                                                                                                                                                                 |
|     `evm`     | Developer utility version of the EVM (Doric Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow isolated, fine-grained debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug run`)
.                                                                                                                                                                                                                                                                     |
| `gethrpctest` | Developer utility tool to support our [Doric/rpc-test](https://github.com/ethereum/rpc-tests) test suite which validates baseline conformity to the [Doric JSON RPC](https://eth.wiki/json-rpc/API) specs. Please see the [test suite's readme](https://github.com/ethereum/rpc-tests/blob/master/README.md) for details.                                                                                                                                                                                                  |
|   `rlpdump`   | Developer utility tool to convert binary RLP ([Recursive Length Prefix](https://eth.wiki/en/fundamentals/rlp)) dumps (data encoding used by the Doric protocol both network as well as consensus wise) to user-friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`).                                                                                                                                                                                                                                 |
|   `puppeth`   | a CLI wizard that aids in creating a new Doric network.                                                                                                                                                                                                                                                                                                                    
# Configuration Doric Proof of Authority 

In this repository your will find a step by step for setup an Doric network.

## Install Go-lang

```
sudo add-apt-repository ppa:longsleep/golang-backports 
sudo apt update 
sudo apt install golang-go
```

## Install Geth + Tools

- Access and realize download all binaries in https://github.com/Doric-Blockchain/doric-blockchain-fullnode/releases

- Create a new folder with name **utils**
- move on all bin to utils

## Setting Files

- Open **Terminal** in Folder of your **Project**

```
mkdir fullnode
```

- Create an new file with name **genesis.json**. Copy and Past this Json.

```json
{
  "config": {
    "chainId": 4141,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "clique": {
      "period": 15,
      "epoch": 30000
    }
  },
  "nonce": "0x0",
  "timestamp": "0x60de7924",
  "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000ba82ed2c5ef98eA216Fc2F5b163c0b10D0b80e752D151ecdcf0E34d8a0595d6B1a8767134028003972c94ab61f1518fe20e50bbb145e164359b837eb0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "gasLimit": "0x47b760",
  "difficulty": "0x1",
  "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "alloc": {
    "c833Ade3205CF59A0E12cfe02a30cC57DDb2B4c3": {
      "balance": "0x00000000000000000000000000000000000000001F04EF12CB04CF158000000"
    }
  },
  "number": "0x0",
  "gasUsed": "0x0",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

### **- Initialize the Nodes**

```
./utils/geth --datadir fullnode init genesis.json 
```


### **- Access fullnode folder**

```
cd fullnode
```

### **- Create static-nodes.json file**

At this stage, you will create a static-nodes.json with all the Enode that was previously created.

```json
[
"enode://77d77f6cc69102e10f6bcb298892e802e711563d0c29e2e349742dc33650abe673699b0ec18d9ba3762c9ab887fe7a3332dd8c0e17e2bc015874335cd4ce5e3b@34.139.140.202:30311"
]
```
### **- Run the Nodes**

```
nohup ./utils/geth --datadir=$pwd --syncmode 'full' --port 30311 --http --http.rpcprefix "/"  --http.corsdomain "*" --http.addr 0.0.0.0 --http.port "8545" --http.api "db, eth, net, web3, personal" --ws --ws.port 8546 --ws.addr 0.0.0.0 --ws.origins "*" --ws.api "web3, eth, personal, net" </dev/null >/dev/null 2>&1 &
```

## License

The go-doric library (i.e. all code outside of the `cmd` directory) is licensed under the
[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html),
also included in our repository in the `COPYING.LESSER` file.

The go-doric binaries (i.e. all code inside of the `cmd` directory) is licensed under the
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html), also
included in our repository in the `COPYING` file.
