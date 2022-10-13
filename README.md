![haqq](https://user-images.githubusercontent.com/94483941/189324668-2e070db6-f0b6-4ddc-bc7c-f613ec404279.png)
<h3>🚨 Genesis update guide haqq_54211-3</h3>
<i>For those who already have a node installed</i>

Stop node
```bash
sudo systemctl stop haqqd
```

Download new genesis
```bash
cd $HOME/.haqqd/config && rm -rf genesis.json && wget https://github.com/haqq-network/validators-contest/raw/master/genesis.json
```

Check genesis.json
```bash
sha256sum $HOME/.haqqd/config/genesis.json
```
sha256sum: b93f2650bdf560cab2cf7706ecee72bfba6d947fa57f8b1b8cb887f8b428233f


Execute unsafe-reset-all
```bash
haqqd tendermint unsafe-reset-all --home=$HOME/.haqqd
```

Run the service file and see the logs of your node
```bash
sudo systemctl start haqqd && \
sudo journalctl -u haqqd -f -o cat
```
