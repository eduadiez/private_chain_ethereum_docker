# private chain ethereum docker
Creation of a private blockchain under ethereum through docker (docker-compose)

# Steps to setup
## Clone repo
```
git clone https://github.com/eduadiez/private_chain_ethereum_docker.git
```

## Init (only the first time)
```
docker-compose -f docker-compose-init.yml up
```

## Start
```
docker-compose up -d
```

## Stop
```
docker-compose stop
```
**BE CAREFOUL! By default it exposes to internt (--rpcaddr "0.0.0.0" --wsaddr "0.0.0.0") so any one can access from outside.**

# Usage
By default it begins to mine in the address "0xabdd735da6639a36fbe02020661e16ec29be214e" (default coinbase)
All of the chain data is saved to 'node' directory
Private keys are save to 'files/keystore' directory

## Version
```
docker exec -ti private-ethereum geth version
Geth
Version: 1.7.0-unstable
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.7.3
Operating System: linux
GOPATH=
GOROOT=/usr/lib/go
```
## Attach to JavaScript console
```
docker exec -ti private-ethereum geth attach
```

## New account
The private key is save to 'files/keystore'
```
docker exec -ti private-ethereum geth account new
```

### JavaScript console examples
```
# Stop mining
> miner.stop()
true

# Start mining 1 thread
> miner.start(1)
null

# Get coinbase
> eth.coinbase
"0xabdd735da6639a36fbe02020661e16ec29be214e"

# Get balance
> web3.fromWei(eth.getBalance(eth.coinbase), "ether");
225

# Get current block
> eth.blockNumber
12

# Send Transaction
personal.unlockAccount(eth.accounts[0],"eduadiez")
personal.unlockAccount(eth.accounts[1],"eduadiez")
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(1, "ether")})
```

### Other examples
Console: `miner.start(4)`

IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":1}' | nc -U $datadir/geth.ipc`

HTTP: `curl -X POST --data '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":74}' localhost:8545`

# The genesis file
```
{
	"config": {
		"chainId": 3,
		"homesteadBlock": 0,
		"eip155Block": 0,
		"eip158Block": 0
	},
	"nonce": "0x0000000000000042",
	"mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
	"difficulty": "0x400",
	"coinbase": "0xabdd735da6639a36fbe02020661e16ec29be214e",
	"timestamp": "0x0",
	"parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
	"extraData": "0x",
	"gasLimit": "0x8000000",
	"alloc": {
		"0xabdd735da6639a36fbe02020661e16ec29be214e" : {"balance": "20000000000000000000"}
	}
}
```
# Connect to mist
```
(Windows) ...\Ethereum Wallet.exe" --rpc http://localhost:8545 
(Mac) /Applications/Ethereum\ Wallet.app/Contents/MacOS/Ethereum\ Wallet --rpc http://localhost:8545
```
# References and documentation
- https://souptacular.gitbooks.io/ethereum-tutorials-and-tips-by-hudson/content/private-chain.html
- https://github.com/Capgemini-AIE/ethereum-docker
- https://github.com/ethereum/go-ethereum
- https://github.com/ethereum/go-ethereum/wiki/Management-APIs
- https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options
- https://etherconverter.online/
