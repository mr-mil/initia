
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
