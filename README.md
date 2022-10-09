# Guide for operating own miningpool with miningcore

## Current state
We operate our own miningpool with software "miningcore" and there are three coins we support now, bitcoin, ethereum and litecoin. Testing connection between mining pool and miner is conducted by software "bfgminer".

There are two servers, xxx.xxx.xxx.35 and xxx.xxx.xxx.36 which will be called as first server(35) and second server(36). Those servers are using ubuntu 18.04.  
First server contains miningcore, bitcoin daemon(and blockchain data) and litecoin daemon(and blockchain data). Second server contains ethereum daemon(and blockchain data).
First server consists of one ssd which includes miningcore and one hhd which includes bitcoin blockchain data. 
Second server consists of two ssd which includes ethereum blockchain data and one hhd which includes ethereum ancient blockchain data.

Bfgminer is installed in the local labtab.

## Guide for Installation

### bitcoind 
1. Install bitcoind 
bitcoin.conf for bitcoin configuration file should be located in /home/user/.bitcoin and below is its content.
```
rpcuser="user"
rpcpassword="password"
rpcbind=0.0.0.0
rpcport=8332
server=1
```
Other configuration can be automatically made in [here](https://jlopp.github.io/bitcoin-core-config-generator/#config=eyJycGMiOnsic2VydmVyIjoxLCJyZXN0IjoxLCJycGNiaW5kIjoiMTI3LjAuMC4xIn19)
After finishing configuration, start syncing bitcoin node with ```bitcoind``` command.

### ethereum
As ethereum [merge](https://blog.ethereum.org/2022/08/24/mainnet-merge-announcement) proceeds, it not only needs execution layer protocl such as geth, but also requires consensus layer protocol such as [prysm](https://github.com/prysmaticlabs/prysm/releases/tag/v3.0.0). 
Those installation is [here](https://docs.prylabs.network/docs/install/install-with-script).

After installing all of the requirements(geth, prysm), it also requires geth syncing and prysm syncing. It requires lots of time(days-weeks).

Below is the command of geth and prysm
```
sudo geth --http --http.port 8545 -http.addr xxx.xxx.xxx.36 --http.api eth,net,engine,admin --authrpc.addr localhost --authrpc.port 8551 --authrpc.vhosts localhost --authrpc.jwtsecret /home/cse/mining_pool/coins/ethereum/consensus/prysm/jwt.hex --datadir ~/.ethereum --datadir.ancient /mnt/sda1/mining_pool/geth --cache=1024 --unlock 0x569D0C9d68E859feC75B3A686A82B6D5CEf82B42 --password /home/cse/.ethereum/keystore/password.txt --allow-insecure-unlock --mine
```
```
./prysm.sh beacon-chain --execution-endpoint=http://localhost:8551 --jwt-secret=/home/cse/mining_pool/coins/ethereum/consensus/prysm/jwt.hex
```

### litecoin
litecoin installation is similiar with bitcoin.
litecoin.conf for litecoin configuraiton file should be located in /home/user/.litecoin and bloe is its content.
```
rpcuser="user"
rpcpassword="password"
rpcbind=0.0.0.0
rpcport=9332
port=9333
server=1
```
After finishing configuration, start syncing litecoin node with ```litecoind``` command.

### bfgminer
[bfgminer](https://github.com/luke-jr/bfgminer) installation guide is [here](https://gist.github.com/vietdien2005/db91091820b00fcf4c9706c6b08e5440)
After installation is finished, use below command to execute bfgminer. 
```
./bfgminer -o stratum+tcp://xxx.xxx.xxx.35:3052 -u 1J74fs5XV1s15YWcJswAa7nRrXHKLe38Mc.001 -p x 
```
```stratum+tcp://xxx.xxx.xxx.35:3052``` is about miningcore's address and port for specific coin that the example is about bitcoin. ```-u 1J74fs5XV1s15YWcJswAa7nRrXHKLe38Mc.001``` is the name of miner which is the wallet address of coin. ```-p x``` is the password of miner.

If you try to connect with other miningpool software such as f2pool, the name of miner should be the name that you sign up with in the miningpool software.

### f2pool 
Guide for mining with f2pool is [here](https://f2pool.io/mining/guides/how-to-mine-bitcoin/?_ga=2.62637100.1249855478.1659249336-888574748.1659249336)

### miningcore
[miningcore](https://github.com/oliverw/miningcore) is the software that can make us operate our own mining pool. It supports lots of coins and frontend api.

Installing miningcore is starting from modifying build-ubuntu-(version).sh file depending on your system. Go through installation stated in the miningcore github.

To configure coins, modify /miningcore/build/config.json file. Its explanation is [here](https://github.com/oliverw/miningcore/wiki/Configuration).
Make sure to configure correctly coin daemon, port and the other variables.

