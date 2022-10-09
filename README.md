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
Other configuration can be automatically made in here(https://jlopp.github.io/bitcoin-core-config-generator/#config=eyJycGMiOnsic2VydmVyIjoxLCJyZXN0IjoxLCJycGNiaW5kIjoiMTI3LjAuMC4xIn19)
After finishing configuration, start syncing bitcoin node with ```bitcoind``` command.

### ethereum

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
bfgminer(https://github.com/luke-jr/bfgminer) installation guide is here(https://gist.github.com/vietdien2005/db91091820b00fcf4c9706c6b08e5440)
After installation is finished, use below command to execute bfgminer. 
```
./bfgminer -o stratum+tcp://xxx.xxx.xxx.35:3052 -u 1J74fs5XV1s15YWcJswAa7nRrXHKLe38Mc.001 -p x 
```
```stratum+tcp://xxx.xxx.xxx.35:3052``` is about miningcore's address and port for specific coin that the example is about bitcoin. ```-u 1J74fs5XV1s15YWcJswAa7nRrXHKLe38Mc.001``` is the name of miner which is the wallet address of coin. ```-p x``` is the password of miner.

If you try to connect with other miningpool software such as f2pool, the name of miner should be the name that you sign up with in the miningpool software.

### f2pool 
Guide for mining with f2pool is here(https://f2pool.io/mining/guides/how-to-mine-bitcoin/?_ga=2.62637100.1249855478.1659249336-888574748.1659249336)
