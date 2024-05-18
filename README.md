# Initia
## Initia Node Kurulum Rehberi
## VPS özellikleri:
| CPU  | 4  |
| RAM  | 16  |
| DISK  | 160 GB  |
---
## Kurulum rehberi
### 1. Gerekli paketleri yükleme
```
sudo apt update && \
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
```
### 2. Go yükleme
```
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
### 3. Initia binary kurulumu
```
git clone https://github.com/initia-labs/initia.git
cd initia
git checkout v0.2.14
make install
initiad version
```

### 4. Değişkenleri ayarlama
```echo 'export MONIKER=Damnational' >> ~/.bash_profile
echo 'export CHAIN_ID=initiation-1' >> ~/.bash_profile
echo 'export WALLET_NAME=Damnational' >> ~/.bash_profile
echo 'export RPC_PORT=26657' >> ~/.bash_profile
source $HOME/.bash_profile
```

### 5. Node'u başlatma
```
cd $HOME
initiad init $MONIKER --chain-id $CHAIN_ID
initiad config set client chain-id $CHAIN_ID
initiad config set client node tcp://localhost:$RPC_PORT
initiad config set client keyring-backend test
```

### 6. genesis.json indirme
`wget https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json -O $HOME/.initia/config/genesis.json`

### 7. Seed ve peer ekleme
```
PEERS="9ea146b73504a8cb2d8269f50b736c1d3e4f54a4@154.12.229.0:53456,421e7eda7975e66fffc0d65da0474dd80a883f6e@185.230.138.64:14656,12cd2a5f22782094cc90470def2fc665050a551d@62.169.20.176:53456,4c7d65bee0ff5fb9ebb3ec8aca477f77a6e30305@194.163.152.237:53456,0864b4f2cafb87dd500feca0a689af2c6381deb3@109.123.251.247:26656,398fca7aab6856631becc4034284c2cdddeed7a6@127.0.0.1:26933,b9b043fb2f836c0dafe9faa287a5f49c4b05cd13@46.38.241.12:53456,0cb2a1a4f900976326d5ff6fadb4d9366fd48a39@149.50.114.176:14656,df76857e532cb93aac68798d805d4460c7765cd1@37.27.108.81:26656,800c76ee5b4787709aba1e7c9e758e6c9c76f583@161.97.104.116:14656,96e561c9d8bc3cf7d039a4f19debb620498736e3@62.169.20.172:53456,57671760b7e2f7d14f23b43559542a4b18dabb4b@38.242.215.207:14656,7d0f01b958c52cdebe2f6704ca69b4dd100a931b@167.86.88.100:14656,1847ef0cb094bde6c305b242f5e9cb740ae7628b@185.252.232.26:14656" && \
SEEDS="2eaa272622d1ba6796100ab39f58c75d458b9dbc@34.142.181.82:26656,c28827cb96c14c905b127b92065a3fb4cd77d7f6@testnet-seeds.whispernode.com:25756" && \
sed -i \
    -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" \
    -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" \
    "$HOME/.initia/config/config.toml"
```
