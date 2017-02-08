#簡介:
以太坊（英語：Ethereum）是一個開源且具有智慧合約區塊鏈平台，設計上是為了解決比特幣擴展性不足的問題，以太坊區塊鏈上的代幣稱為以太幣（Ether），代碼為ETH

合約:

>合約是一個活在以太坊系統內的自動代理人，他有一個自己的乙太幣地址，當用戶向合約的地址發送一筆交易後，該合約就被激活，然後根據交易中的額外信息，合約會運行自身的代碼，最後返回一個結果，這個結果可能是從合約的地址發出另外一筆交易


#安裝:

這邊我們會使用`Docker`，並使用ethereum相關的container來進行
https://hub.docker.com/r/ethereum/client-go/

所以須先安裝好`Docker`https://docs.docker.com/engine/installation/
並且執行

再來開啟terminal

1.加入以太坊的public network
```
docker run -d --name ethereum-node \
    -v $HOME/.ethereum:/root \
    -p 8545:8545 -p 30303:30303 \
    ethereum/client-go --fast --cache=512
```
執行attach
```
docker exec -ti ethereum-node geth attach
```
2.加入以太坊的test network
```
docker run -d --name ethereum-node \
    -v $HOME/.ethereum:/root \
    -p 8545:8545 -p 30303:30303 \
    ethereum/client-go --testnet --fast --cache=512
```
執行attach

```
docker exec -ti ethereum-node \
    geth attach ipc:/root/.ethereum/testnet/geth.ipc
```

查看LOG
```
docker logs ethereum-node
```


>attach的意思是在terminal啟動一個javascript的相關以太坊API執行環境
https://github.com/ethereum/wiki/wiki/JavaScript-API


但我們這邊主要教學部分為Private chain，所以我們下載一個專門給ethereum private chain 使用的docker container

```
docker pull vertigo/ethereum
```
https://github.com/vertigobr/ethereum

我們可以到他的repo中點選`Dockerfile`

看到

```
ENV GEN_NONCE="0xeddeadbabeeddead" \

    NET_ID=1981
```


這兩個分別是創世區塊的識別ID與private network的識別ID，所以未來加入其他節點這兩個都要與其他節點的值相同才會加入，預設我們不用去改他


我們也可以用`-e`來指定執行docker時的環境變數
```
docker run -e GEN_NONCE='stag0x2222ing' 
```

講解其中的一些.sh程式
```
bootnode.sh: 執行Ethereum bootnode 啟動節點;

runnode.sh: 執行Ethereum 非挖礦節點;

runminer.sh: 執行Ethereum 挖礦節點;

showpeers.sh: 顯示所有連線到此節點的節點

killall.sh: 將會執行"docker stop" 與 "docker rm" ;

wipeall.sh: 將會執行"docker stop" 與 "docker rm" 並且清空 volume folders.
```

#常見名詞:

Dapp:
>A Dapp (‘decentralized app’) consists of two parts: a frontend, written in HTML, and a backend (think of it as the ‘database’ for your frontend).

