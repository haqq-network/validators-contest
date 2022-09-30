haqq
Prepare to launch intensivized testnet Haqq
🟢 Instructions for those who do not transfer node to another server
Update binary haqqd to v1.1.0

cd $HOME/haqq && \
git fetch && \
git checkout v1.1.0 && \
make install && \
haqqd version --long | head

name: haqq
server_name: haqqd
version: '"1.1.0"'
commit: 58215364d5be4c9ab2b17b2a80cf89f10f6de38a
...
Remove old genesis and download genesis.json to your server in .haqqd folder

rm -rf $HOME/.haqqd/config/genesis.json && cd $HOME/.haqqd/config/ && wget https://raw.githubusercontent.com/haqq-network/validators-contest/master/genesis.json
Check genesis.json

sha256sum $HOME/.haqqd/config/genesis.json
8c79dda3c8f0b2b9c0f5e770136fd6044ea1a062c9272d17665cb31464a371f7
Create a service file

sudo tee /etc/systemd/system/haqqd.service > /dev/null <<EOF
[Unit]
Description=Haqq Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which haqqd) start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
Insertion of peers

seeds="62bf004201a90ce00df6f69390378c3d90f6dd7e@seed2.testedge2.haqq.network:26656,23a1176c9911eac442d6d1bf15f92eeabb3981d5@seed1.testedge2.haqq.network:26656"
peers="b3ce1618585a9012c42e9a78bf4a5c1b4bad1123@65.21.170.3:33656,952b9d918037bc8f6d52756c111d0a30a456b3fe@213.239.217.52:29656,85301989752fe0ca934854aecc6379c1ccddf937@65.109.49.111:26556,d648d598c34e0e58ec759aa399fe4534021e8401@109.205.180.81:29956,f2c77f2169b753f93078de2b6b86bfa1ec4a6282@141.95.124.150:20116,eaa6d38517bbc32bdc487e894b6be9477fb9298f@78.107.234.44:45656,37513faac5f48bd043a1be122096c1ea1c973854@65.108.52.192:36656,d2764c55607aa9e8d4cee6e763d3d14e73b83168@66.94.119.47:26656,fc4311f0109d5aed5fcb8656fb6eab29c15d1cf6@65.109.53.53:26656,297bf784ea674e05d36af48e3a951de966f9aa40@65.109.34.133:36656,bc8c24e9d231faf55d4c6c8992a8b187cdd5c214@65.109.17.86:32656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.haqqd/config/config.toml
Run the service file and see the logs of your node

sudo systemctl daemon-reload && \
sudo systemctl enable haqqd && \
sudo systemctl restart haqqd && \
sudo journalctl -u haqqd -f -o cat
If you run a node at the beginning of the testnet and you are on genesis, you will get this message

Genesis time is in the future. Sleeping until then... genTime=...
🔴 Instructions for those who transfer the node to another server
If you want to move to another server - make sure you have saved the mnemonic and priv_validator_key.json from the server where you did the gentx

Update packages and install required packages

sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
Install Go 1.18.3

wget https://golang.org/dl/go1.18.3.linux-amd64.tar.gz; \
rm -rv /usr/local/go; \
tar -C /usr/local -xzf go1.18.3.linux-amd64.tar.gz && \
rm -v go1.18.3.linux-amd64.tar.gz && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version > /dev/null
Install binary project

cd $HOME && git clone https://github.com/haqq-network/haqq && \
cd haqq && \
git checkout v1.1.0 && \
make install && \
haqqd version --long | head

name: haqq
server_name: haqqd
version: '"1.1.0"'
commit: 58215364d5be4c9ab2b17b2a80cf89f10f6de38a
...
Also init your node

haqqd init <YOURMONIKER> --chain-id haqq_54211-2 && \
haqqd config chain-id haqq_54211-2
Recover your wallet

haqqd keys add <YOURWALLET> --recover
📥 Upload the saved priv_validator_key.json to your server. The path should look like this /.haqqd/config/priv_validator_key.json

Remove old genesis.json and download genesis.json to your server in .haqqd folder

rm -rf $HOME/.haqqd/config/genesis.json && cd $HOME/.haqqd/config/ && wget https://raw.githubusercontent.com/haqq-network/validators-contest/master/genesis.json
Check genesis.json

sha256sum $HOME/.haqqd/config/genesis.json
8c79dda3c8f0b2b9c0f5e770136fd6044ea1a062c9272d17665cb31464a371f7
Create a service file

sudo tee /etc/systemd/system/haqqd.service > /dev/null <<EOF
[Unit]
Description=Haqq Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which haqqd) start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
Insertion of peers

seeds="62bf004201a90ce00df6f69390378c3d90f6dd7e@seed2.testedge2.haqq.network:26656,23a1176c9911eac442d6d1bf15f92eeabb3981d5@seed1.testedge2.haqq.network:26656"
peers="b3ce1618585a9012c42e9a78bf4a5c1b4bad1123@65.21.170.3:33656,952b9d918037bc8f6d52756c111d0a30a456b3fe@213.239.217.52:29656,85301989752fe0ca934854aecc6379c1ccddf937@65.109.49.111:26556,d648d598c34e0e58ec759aa399fe4534021e8401@109.205.180.81:29956,f2c77f2169b753f93078de2b6b86bfa1ec4a6282@141.95.124.150:20116,eaa6d38517bbc32bdc487e894b6be9477fb9298f@78.107.234.44:45656,37513faac5f48bd043a1be122096c1ea1c973854@65.108.52.192:36656,d2764c55607aa9e8d4cee6e763d3d14e73b83168@66.94.119.47:26656,fc4311f0109d5aed5fcb8656fb6eab29c15d1cf6@65.109.53.53:26656,297bf784ea674e05d36af48e3a951de966f9aa40@65.109.34.133:36656,bc8c24e9d231faf55d4c6c8992a8b187cdd5c214@65.109.17.86:32656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.haqqd/config/config.toml

Add Addressbook

wget -O $HOME/.haqqd/config/addrbook.json "https://raw.githubusercontent.com/StakeTake/guidecosmos/main/haqq/haqq_54211-2/addrbook.json"

Run the service file and see the logs of your node

sudo systemctl daemon-reload && \
sudo systemctl enable haqqd && \
sudo systemctl restart haqqd && \
sudo journalctl -u haqqd -f -o cat
If you run a node at the beginning of the testnet and you are on genesis, you will get this message

Genesis time is in the future. Sleeping until then... genTime=...
