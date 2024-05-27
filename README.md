
<h1>Complete Guide to Node Validator of the Initia project</h1>
<h2>Recommended System :</h2>
<ul>
<li>RAM 16GB</li>
<li>CPU 4 Core</li>
<li>Disk 1TB SSD</li>
<li>OS Ubuntu 20</li>
</ul>

<h3>1. Dependecies :</h3>

```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
```

<h3>2. Install GO :</h3>

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

<h3>3. Download and install the initia library :</h3>

```
git clone https://github.com/initia-labs/initia.git
```

```
cd initia
```

```
git checkout v0.2.15
```

```
git switch -c v0.2.15
```

```
make install
```

```
initiad version
```

<h3>4. Set the Default Values of Node :</h3>

<p><strong>Before running the following code, make sure that there is the word "Moniker" in the first line and you should replace the default name with your desired name.</strong></p>
<p>The default name here is my_node.</p>
<p>This will be your node name.</p>

```
echo 'export MONIKER="my_node"' >> ~/.bash_profile
echo 'export CHAIN_ID="initiation-1"' >> ~/.bash_profile
echo 'export WALLET_NAME="wallet"' >> ~/.bash_profile
echo 'export RPC_PORT="26657"' >> ~/.bash_profile  
source $HOME/.bash_profile
```

```
cd $HOME
initiad init $MONIKER --chain-id $CHAIN_ID
initiad config set client chain-id $CHAIN_ID
initiad config set client node tcp://localhost:$RPC_PORT
initiad config set client keyring-backend os
```

```
wget https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json -O $HOME/.initia/config/genesis.json
```

<strong>Note: </strong> Always get new SEEDS and PEERS from project discord.

<H3>Set PEERS and SEEDS :</H3>

```
PEERS="0aa35c1eaa0933218089b5fb1876e2d46d8f2240@37.60.229.199:27656,ebefecfd04824cfbd674bd4c0fcfd9315aadc210@103.170.155.158:27656,f9c8aa40fc2dad2c379316e33e4e855a0b40b2c7@109.199.109.89:27656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:17956,7e07c1d69e3f726d985614728c601b759f008e52@213.199.57.83:27656,818637c0b4fa7d9ced2283a4853b3f33fc55bb2d@95.111.224.8:26656,57fd42ad4b6d70868e645f17d846d7a6833404a9@194.180.188.44:27656,fc37e22ae9405cf00a775a014366d428376e47b3@37.27.48.77:29656,4701e15068475260a267aabe9b4df8addbb1f992@144.91.79.216:26656,96a259e4cbcde828faed933179784614cd0ea8d1@185.130.226.218:27656,bb37fee58e592edbe8acb627be937d9d17f0ef59@129.146.80.192:27656,6c47bb7c20367964f79755be5bfd4a06c50bae5a@159.89.11.119:27656,7b8ef017be344d202f65628281f5a166dafb6ec0@213.199.34.19:27656,14a53cee4fded2635149228e92b7a752971c40fb@185.130.226.121:27656,616900b4fc44040e4539821fa986ddb85b6f7b87@77.237.238.175:27656,7a98741ef8f7f42113779ccd952635b5a43a7d4f@192.3.128.80:27656,4f9b24bbe1d8a108f4f856d4125a7a6457b1b8cd@142.132.199.231:27656,a1de81504903d857695804f34e5bc1c1b9fc734a@84.46.242.232:27656,9fb117a08ac24032bca52914bb089e0a8df5a239@185.130.227.28:27656,fc935f2f3e3cfebef7d73abc9958a46b308366f4@109.199.122.112:27656,fe19d84d88e615dc100bac74dd743400b4d082d1@195.26.249.58:27656" && \
SEEDS="2eaa272622d1ba6796100ab39f58c75d458b9dbc@34.142.181.82:26656,c28827cb96c14c905b127b92065a3fb4cd77d7f6@testnet-seeds.whispernode.com:25756" && \
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.initia/config/config.toml
```

```
EXTERNAL_IP=$(wget -qO- eth0.me) \
PROXY_APP_PORT=26658 \
P2P_PORT=26656 \
PPROF_PORT=6060 \
API_PORT=1317 \
GRPC_PORT=9090 \
GRPC_WEB_PORT=9091
```

```

sed -i \
    -e "s/\(proxy_app = \"tcp:\/\/\)\([^:]*\):\([0-9]*\).*/\1\2:$PROXY_APP_PORT\"/" \
    -e "s/\(laddr = \"tcp:\/\/\)\([^:]*\):\([0-9]*\).*/\1\2:$RPC_PORT\"/" \
    -e "s/\(pprof_laddr = \"\)\([^:]*\):\([0-9]*\).*/\1localhost:$PPROF_PORT\"/" \
    -e "/\[p2p\]/,/^\[/{s/\(laddr = \"tcp:\/\/\)\([^:]*\):\([0-9]*\).*/\1\2:$P2P_PORT\"/}" \
    -e "/\[p2p\]/,/^\[/{s/\(external_address = \"\)\([^:]*\):\([0-9]*\).*/\1${EXTERNAL_IP}:$P2P_PORT\"/; t; s/\(external_address = \"\).*/\1${EXTERNAL_IP}:$P2P_PORT\"/}" \
    $HOME/.initia/config/config.toml
```

```
sed -i \
    -e "/\[api\]/,/^\[/{s/\(address = \"tcp:\/\/\)\([^:]*\):\([0-9]*\)\(\".*\)/\1\2:$API_PORT\4/}" \
    -e "/\[grpc\]/,/^\[/{s/\(address = \"\)\([^:]*\):\([0-9]*\)\(\".*\)/\1\2:$GRPC_PORT\4/}" \
    -e "/\[grpc-web\]/,/^\[/{s/\(address = \"\)\([^:]*\):\([0-9]*\)\(\".*\)/\1\2:$GRPC_WEB_PORT\4/}" \
    $HOME/.initia/config/app.toml
```

```
sed -i \
    -e "s/^pruning *=.*/pruning = \"custom\"/" \
    -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" \
    -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" \
    "$HOME/.initia/config/app.toml"
```

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.15uinit,0.01uusdc\"/" $HOME/.initia/config/app.toml
```

```
sed -i "s/^indexer *=.*/indexer = \"kv\"/" $HOME/.initia/config/config.toml
```

```
sudo tee /etc/systemd/system/initiad.service > /dev/null <<EOF
[Unit]
Description=Initia Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which initiad) start --home $HOME/.initia
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

<h3>Restart the system and node to set the config:</h3>

```
sudo systemctl daemon-reload && \
sudo systemctl enable initiad && \
sudo systemctl restart initiad && \
sudo journalctl -u initiad -f -o cat
```

<h3>Snapshot Download :</h3>
In order for the node to sync to the network faster, you need to download the latest snapshot.

```
sudo apt update
```

```
sudo apt install snapd -y
```

```
sudo snap install lz4
```

```
sudo systemctl stop initiad
```

```
wget -O initia_202803.tar.lz4 https://snapshots.polkachu.com/testnet-snapshots/initia/initia_202803.tar.lz4 --inet4-only
```

```
sudo systemctl restart initiad
```

```
cp ~/.initia/data/priv_validator_state.json  ~/.initia/priv_validator_state.json
```

```
initiad tendermint unsafe-reset-all --home $HOME/.initia --keep-addr-book
```

```
mv $HOME/.initia/priv_validator_state.json.backup $HOME/.initia/data/priv_validator_state.json
```

```
lz4 -c -d initia_202803.tar.lz4  | tar -x -C $HOME/.initia
```

```
cp ~/.initia/priv_validator_state.json  ~/.initia/data/priv_validator_state.json
```

```
sudo systemctl restart initiad && sudo journalctl -u initiad -f -o cat
```

Press ctrl+c to close this window. </br>
Node is now running.</br>
You have to wait until it is completely synced with the blocks on the network.</br>
You can check the sync status of your node by running the following command:

```
initiad status | jq
```

When the status changes to false, it means that the sync is complete. </br>
Now it's time to build the wallet and validator.</br>

If you want to create a new wallet, run the following code:

```
initiad keys add $WALLET_NAME
```

If you want to recovery your previous wallet, run the following code:

```
initiad keys add --recover $WALLET_NAME
```

And then enter your seed phrase. </br></br>

Now get the faucet through the following address and deposit to the relevant wallet address:

<a href="https://faucet.testnet.initia.xyz">https://faucet.testnet.initia.xyz</a> </br>

You can view your wallet balance with the following command:

```
initiad q bank balances $(initiad keys show $WALLET_NAME -a)
```
