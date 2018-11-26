# Running an Ontology Node

With the Ontology CLI, you will be able to an Ontology node an various different configurations. Depending on the combination of parameters passed to the base `./Ontology` script, you can run everything from a main net consensus node to a local private net that will run with just your single node. There are a wide variety of parameter combinations that can be used, but first we will go over the most common, and you can proceed to customize your configuration from there.

* [Types of Nodes](#types-of-nodes)
	* [MainNet Bookkeeping Node Deployment](#mainnet-bookkeeping-node-deployment)
	* [MainNet Synchronization Node Deployment](#mainnet-synchronization-node-deployment)
	* [Deploying on public test network Polaris sync node](#deploying-on-public-test-network-polaris-sync-node)
	* [Single-Node Test Network Deployment](#single-node-test-network-deployment)
* [Additional Parameters](#additional-parameters)
	* [Ontology System Parameters](#ontology-system-parameters)
	* [Account Parameters](#account-parameters)
	* [Consensus Parameters](#consensus-parameters)
	* [P2P Network Parameters](#p2p-network-parameters)
	* [RPC Server Parameters](#rpc-server-parameters)
	* [RESTful Server Parameters](#restful-server-parameters)
	* [Web Socket Server Parameters](#web-socket-server-parameters)
	* [Test Mode Parameters](#test-mode-parameters)
	* [Transaction Parameters](#transaction-parameter)


## Types of Nodes

The two different types of nodes that can be run are bookkeeping nodes and synchronization nodes. Bookkeeping nodes participate in the network consensus, and must be must be voted in be user staking of ONT, after applying to be a consensus node. More information on Ontology consensus can be found [here](https://medium.com/ontologynetwork/triones-node-incentive-model-dbcb175f4728).

In contrast to bookkeeping nodes, synchronization nodes to not participate in network consensus, and only synchronize the blocks generated by the bookkeeping nodes. Running the base `./Ontology` command with no additional details will start up a node in synchronization mode.


### MainNet Bookkeeping Node Deployment

Recommended bookkeeping node startup parameters:

```
./Ontology --enable-consensus --disable-rpc --disable-event-log
```
    - `enable-consensus` is use to start the consensus
    - `disable-rpc` is to close the rpc services for the safe concerns.
    - `disable-event-log` is to disable the event log for high performance.

By default, the script will look for your `wallet.dat` file at `./wallet.dat`, with relation to where the `./Ontology` script is held. To change this, please use the `--wallet` command (eg. `--wallet ./myWallet/wallet.dat`).

All nodes in the same network need to reference the same genesis block config file. By default, this will be pulled from the Ontology Polaris testnet. If you need to reference a specific file, you can use the `--config` parameter (eg. `--config ./configFiles/mainNetConfig`)

If at any time the bookkeeping node needs to adjust the ONG gas price for the transaction pool a combination of the `--gasprice` and `--gaslimit` parameters can be used.


### MainNet Synchronization Node Deployment

The synchronization nodes to not participate in the network consensus, and just sync the blocks that are generated by the bookkeeping nodes. This is the default script mode.

```
./Ontology
```

Since consensus won't be turned on, you don't need a wallet when starting up a synchronization node.

If the node does not use the default genesis block configuration file, it can be specified with the --config parameter.


### Deploying on public test network Polaris Synchronization node

You can also run a synchronization node on the Polaris testnet. In order to do this, just add the `--networkid` parameter with the value `2`.

```
./Ontology --networkid 2
```

### Single-Node Test Network Deployment

Ontology supports single-node network deployment for the development of test environments. To start a single-node test network, you only need to add the --testmode command line parameter.

```
./Ontology --testmode
```

Just as with main net bookkeeping nodes, the script will look for your `wallet.dat` file at `./wallet.dat`, with relation to where the `./Ontology` script is held. To change this, please use the `--wallet` command (eg. `--wallet ./myWallet/wallet.dat`).

If you need to reference a specific file, you can use the `--config` parameter (eg. `--config ./configFiles/privateNetConfig`)

If any time the bookkeeping node need to adjust the ONG gas price for the transaction pool a combination of the `--gasprice` and `--gaslimit` parameters can be used.

When in test mode the node will automatically turn consensus, RPC, RESTful, and WebSocket server options.


## Additional Parameters

In addition to the parameters mentioned above, there are many more configuration parameters available.

You can run `./Ontology --help` in the cli to get the full list of commands.

### Ontology System Parameters

#### `--config`
The config parameter specifies the file path of the genesis block for the current Ontology node. If not specified, Ontology will use the config of Polaris TestNet. Note that the genesis block configuration must be the same for all nodes in the same network, otherwise it will not be able to synchronize blocks or start nodes due to block data incompatibility.

#### `--loglevel`
Used to set the log level the Ontology outputs. Ontology supports 7 different log levels.

	0: Trace (All events are logged)
	1: Debug
	2: Info (Default)
	3: Warn
	4: Error
	5: Fatal
	6: MaxLevel (Only the most critical of errors are logged)

#### `--disable-event-log`
Used to disable the event log output when the smart contract is executed to improve the node transaction execution performance. The Ontology node enables the event log output function by default.

#### `--data-dir`
The data-dir parameter specifies the path where block data will be stored. The default value is "./Chain".


### Account Parameters

#### `--wallet` or `-w`
Used to specify the file path of you wallet file to use for your Ontology node in bookkeeping mode. The default value is "./wallet.dat".

#### `--account` or `-a`
Used to specify the account address to use for your Ontology node in bookkeeping mode. If the account is null, it uses the wallet default account.

#### `--password` or `-p`
Used to specify the account password to use for your Ontology node in bookkeeping mode. Because the account password entered in the command line is saved in the log, it is easy to leak the password. Therefore, it is not recommended to use this parameter in a production environment.


### Consensus Parameters

#### `--enable-consensus`
Used to start a Ontology node in bookkeeping mode. The default is disable.

#### `--max-tx-in-block`
Used to set the maximum transaction number of a block. The default value is 50000.


### P2P Network Parameters

#### `--networkid`
Used to specify the network ID. Different network ids cannot connect to the blockchain network.

	1: main net
	2: polaris test net
	3: testmode
	other: custom network.

#### `--nodeport`
Used to specify the P2P network port number. The default value is 20338.

#### `--consensusport`
Specifies the consensus network port number. By default, the consensus network reuses the P2P network, so it is not necessary to specify a consensus network port. After the dual network is enabled with the --dual-port parameter, the consensus network port number must be set separately. The default is 20339.

#### `--dual-port`
Initiates a dual network, i.e. a P2P network for processing transaction messages and a consensus network for consensus messages. The parameter disables by default.


### RPC Server Parameters

#### `--disable-rpc`
Used to shut down the EPC server. The Ontology node starts the RPC server by default at startup.

#### `--rpcport`
Specifies the port number to which the RPC server is bound. The default is 20336.


### RESTful Server Parameters

#### `--rest`
Used to start the RESTful server.

#### `--restport`
Specifies the port number to which the RESTful server is bound. The default value is 20334.


### WebSocket Server Parameters

#### `--ws`
Used to start the WebSocket server.

#### `--wsport`
Specifies the port number to which the WebSocket server is bound. The default value is 20335


### Test Mode Parameters

#### `--testmode`
Used to start a single node test network for ease of development and debug. Ontology node will start RPC, RESTful and WebSocket servers, and blockchain data continue from any last start in testmode.

#### `--testmode-gen-block-time`
Used to set the block-out time in testmode. The time unit is in seconds, and the minimum block-out time is 2 seconds.


### Transaction Parameters

#### `--gasprice`
Used to set the lowest gasprice of the current node transaction pool to accept transactions.

Transactions below this gasprice will be discarded.

The default value is 500 (0 in testmode).

#### `--gaslimit`
Used to set the gaslimit of the current node transaction pool to accept transactions.

Transactions below this gaslimit will be discarded.

The default value is 20000.

#### `--disable-tx-pool-pre-exec`
Used to disable preExecution of a transaction from network in the transaction pool.

By default, preExecution is enabled when ontology bootstrap.

#### `--disable-sync-verify-tx`
Used to disable sync verify transaction in send transactions, include rpc restful websocket.

#### `--disable-broadcast-net-tx`
Used to disable broadcast a transaction from network in the transaction pool.

By default, this function is enabled when ontology bootstrap.