![haqq (1)](https://user-images.githubusercontent.com/104348282/188024190-b43f56d0-2dc6-4e4a-be0e-a7e9f615f751.png)
# Prepare intensivized testnet Haqq
*Instructions on how to prepare for the testnet*

**Update packages and install required packages**
```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

**Install Go 1.18.3**
```bash
wget https://golang.org/dl/go1.18.3.linux-amd64.tar.gz; \
rm -rv /usr/local/go; \
tar -C /usr/local -xzf go1.18.3.linux-amd64.tar.gz && \
rm -v go1.18.3.linux-amd64.tar.gz && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version > /dev/null
```

**Install binary project**
```bash
cd $HOME && git clone https://github.com/haqq-network/haqq && \
cd haqq && \
make install && \
haqqd version
```

**Init moniker and set chainid**
```bash
haqqd init YOURMONIKER --chain-id haqq_54211-2 && \
haqqd config chain-id haqq_54211-2
```

**Create wallet**
```bash
haqqd keys add YOURWALLETNAME
```

**Add genesis account**
```bash
haqqd add-genesis-account YOURWALLETNAME 10000000000000000000aISLM
```

**Create gentx**
```bash
haqqd gentx YOURWALLETNAME 10000000aISLM \
--chain-id=haqq_54211-2 \
--moniker="YOURMONIKERNAME" \
--commission-max-change-rate 0.05 \
--commission-max-rate 0.20 \
--commission-rate 0.05 \
--website="" \
--security-contact="" \
--identity="" \
--details=""
```

After executing this command, you have a gentx. Submit a pull request (gentx folder) with the given gentx
```bash
File Genesis transaction written to "/.haqqd/config/gentx/gentx-xxx.json"
```







