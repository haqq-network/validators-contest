![haqq (1)](https://user-images.githubusercontent.com/104348282/188024190-b43f56d0-2dc6-4e4a-be0e-a7e9f615f751.png)
# Intensivized Testnet Haqq


**Auto Install Haqqd V1.0.3**
```bash
wget -O upgrade%20haqqd%20v1.0.3.sh https://raw.githubusercontent.com/fatalbar/Testnet-validator/main/Haqq%20intensivized%20testnet/upgrade%20haqqd%20v1.0.3.sh && chmod +x upgrade%20haqqd%20v1.0.3.sh && ./upgrade%20haqqd%20v1.0.3.sh
```

**Make Bash Profile**
```bash
source $HOME/.bash_profile
```

**State Sync**
```bash
sudo systemctl stop haqqd
haqqd tendermint unsafe-reset-all --home ~/.haqqd
peers="0833039f717227ccd156d156ea772746b8ac6d71@146.19.24.139:42656"; \
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.haqqd/config/config.toml
SNAP_RPC="http://146.19.24.139:42657"; \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash); \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.haqqd/config/config.toml
sudo systemctl restart haqqd && journalctl -u haqqd -f -o cat
```

**Check Status Sync**
```bash
haqqd status 2>&1 | jq .SyncInfo

```

**Create wallet**
```bash
haqqd keys add $WALLET
```

**Recover Old Wallet**
```bash
haqqd keys add $WALLET --recover
```

**Check Wallet**
```bash
haqqd keys list
```

Save your wallet on bash Profile
```bash
HAQQ_WALLET_ADDRESS=$(haqqd keys show $WALLET -a)
HAQQ_VALOPER_ADDR
ESS=$(haqqd keys show $WALLET --bech val -a)
echo 'export HAQQ_WALLET_ADDRESS='${HAQQ_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export HAQQ_VALOPER_ADDRESS='${HAQQ_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

You need private keys to export your wallet to Metamask
```bash
haqqd keys unsafe-export-eth-key $WALLET
```
Copy your private keys then import to Metamask,now you can Claim faucet There https://testedge2.haqq.network/

Check Balance
```bash
haqqd query bank balances $HAQQ_WALLET_ADDRESS
```

Make sure your status of node must be catching up false and your wallet has funded you can check your status sync
```bash
haqqd status 2>&1 | jq .SyncInfo

```
CREATE VALIDATOR
```bash
haqqd tx staking create-validator \
  --amount 1000000000000000000aISLM \
  --from $WALLET \
  --commission-max-change-rate "0.05" \
  --commission-max-rate "0.20" \
  --commission-rate "0.05" \
  --min-self-delegation "1" \
  --pubkey $(haqqd tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id haqq_54211-2 \
  --gas 300000 \
  -y

```
After Create Validator you can check >> https://haqq.explorers.guru/


Edit Validator
```bash
haqqd tx staking edit-validator \
--moniker="<Yournodename>" \
--identity="<your_keybase_id>" \
--details="<your_validator_description>" \
--chain-id=haqq_54211-2 \
--from=$WALLET \
--gas=auto \
-y 
```

Unjail Validator
```bash
haqqd tx slashing unjail \
  --broadcast-mode=block \
  --from=$WALLET \
  --chain-id=haqq_54211-2
```

Delegate and Stake to your Validator
```bash
haqqd tx staking delegate $HAQQ_VALOPER_ADDRESS 1ISLM --from=$WALLET --chain-id=haqq_54211-2
```
Reedem All Reward
```bash
haqqd tx distribution withdraw-all-rewards --from=$WALLET --chain-id=haqq_54211-2
```

Useful Command

Sync Info
```bash
haqqd status 2>&1 | jq .SyncInfo
```

Check Log
```bash
journalctl -fu haqqd -o cat
```

Validator Info
```bash
haqqd status 2>&1 | jq .ValidatorInfo
```
Node Info
```bash
haqqd status 2>&1 | jq .NodeInfo
```
Node ID
```bash
haqqd tendermint show-node-id
```
Start Service
```bash
 sudo systemctl start haqqd
```

Stop Service
```bash
 sudo systemctl stop haqqd
```

Restart Service
```bash
 sudo systemctl restart haqqd
```

Delete Node
```bash
sudo systemctl stop haqqd
sudo systemctl disable haqqd
sudo rm /etc/systemd/system/haqq* -rf
sudo rm $(which haqqd) -rf
sudo rm $HOME/.haqqd* -rf
sudo rm $HOME/haqq -rf
sed -i '/HAQQ_/d' ~/.bash_profile
```
