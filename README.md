# Avail
Avail Node Installation Instructions </br>
### [Official documentation](https://docs.availproject.org/category/operate-a-node/)

### System requirements: </br>
CPU: 2 Core </br>
RAM: 4 Gb </br>
SSD: 50 Gb </br>
OS: Ubuntu 22.04 LTS </br>
    
# Manual configuration of the Lava node with Cosmovisor
Required packages installation </br>
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl git jq lz4 build-essential
sudo apt install -y unzip logrotate git jq sed wget curl coreutils systemd
sudo apt install make clang pkg-config libssl-dev build-essential git screen protobuf-compiler -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop tar ncdu unzip -y
```

### Download and build binaries
```
cd $HOME
mkdir $HOME/.avail && cd $HOME/.avail
wget https://github.com/availproject/avail/releases/download/v1.9.0.0/x86_64-ubuntu-2204-data-avail.tar.gz && tar -xvf x86_64-ubuntu-2204-data-avail.tar.gz
rm -rf x86_64-ubuntu-2204-data-avail.tar.gz
mv data-avail /usr/bin/avail
avail --version
```
### Creating servicers
```
sudo tee /etc/systemd/system/avail.service > /dev/null <<EOF
[Unit]
Description=AvailNode
After=network-online.target

[Service]
User=$USER
ExecStart=$(which avail) -d /home/avail/data --chain goldberg --validator --name YOUR_NICKNAME
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Launching the service file
```
sudo systemctl daemon-reload && sudo systemctl enable avail && sudo systemctl restart avail
sudo journalctl -u avail -f -o cat
```

You can check your node in the [telemetry](https://telemetry.avail.tools/#list/0x6f09966420b2608d1947ccfb0f2a362450d1fc7fd902c29b67c906eaa965a7ae) here

Once the node is synchronized, use the following command to get the id
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
The output will be as follows:
{"{"jsonrpc":"2.0","result":"0 x..............."," id":1} Our id starts with 0x. We save it, we will need it further.
```

### Creating an account
```
Go to the [website](https://goldberg.avail.tools/)
Go to the Accounts tab
Creating a new account, do not forget to save the seed phrase. We register the account name and password. Next, the json file is downloaded, do not forget to save it as well!
Next, copy the wallet address and go to the server [discord](https://discord.gg/availproject)
In the #goldberg-faucet channel, we write the command:
/deposit "your address"
The channel appears if you go through the gitkin passport in the #faucet access branch and gain more than 20 points.
After a while, the tokens will be credited to your wallet.
Next, go to the staking [tab](https://goldberg.avail.tools/#/staking/actions). Click on the stash icon
We specify the number of funds to block and where the reward will be sent. Next, click the bond button and then sign the transaction.
In the next step, you will have the opportunity to specify the Session Key, use the id received after syncing the node and sign the transaction.
The next step will be the Validate button. Here we specify the percentage of the commission.
If you have enough steak, the validator will start
```

### Updating the node
```
The update did not come out, as soon as it appears, we will immediately update the guide
```

### Deleting a node
```
systemctl stop avail
systemctl disable avail
rm -rf /etc/systemd/system/avail.service
cd $HOME
rm -rf .avail
```
