# Selfchain node

### 1.Update system
```
sudo apt update
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y
```

### 2.Install Go
```
sudo apt install golang  
sudo apt-get update  
sudo apt-get -y upgrade  

wget  https://go.dev/dl/go1.20.5.linux-amd64.tar.gz 

sudo tar -xvf go1.20.5.linux-amd64.tar.gz   
rm -rf /usr/local/go
sudo mv go /usr/local  

export GOROOT=/usr/local/go 

export GOPATH=$HOME/Projects/Proj1 

export PATH=$GOPATH/bin:$GOROOT/bin:$PATH 

go version 

```
### 3.Install node

```
cd $HOME
mkdir -p /root/go/bin/
```
```
wget https://ss-t.self.nodestake.top/selfchaind
```
```
chmod +x selfchaind
```
```
mv selfchaind /root/go/bin/
```
```
selfchaind version 
```
### 4. Initialize Node
##### Replace NodeName with your own moniker.

```
selfchaind init NodeName --chain-id=self-dev-1
```

##### Download Genesis
```
curl -Ls https://github.com/Hoanghienvi/Selfchain/blob/main/genesis.json > $HOME/.selfchain/config/genesis.json
```
##### Download addrbook
```
curl -Ls https://github.com/Hoanghienvi/Selfchain/blob/main/addrbook.json > $HOME/.selfchain/config/addrbook.json
```
##### Create Service
```
sudo tee /etc/systemd/system/selfchaind.service > /dev/null <<EOF
[Unit]
Description=selfchaind Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which selfchaind) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable selfchaind
```
##### Launch Node
```
sudo systemctl restart selfchaind
```
##### Check synch
```
journalctl -u selfchaind -f
```
##### Check logs
```
tail -f selfchain.out
```
