<h1 align="center"> Empower Chain </h1>


Topluluk [Empower Discord](https://discord.gg/uxD8n5gK)

<h1 align="center"> Donanım </h1>

```sh
# Sistem gereksinimleri ;
4 CPU
8 RAM
200 SSD
```

<h1 align="center"> sunucu güncellemesi yapalım </h1>

```
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

<h1 align="center"> Go kurulumu </h1>

```sh
sudo rm -rf /usr/local/go
wget https://golang.org/dl/go1.20.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
export PATH=/usr/local/go/bin:$PATH

# go version 1.20 olmak zorunda
go version
```

<h1 align="center"> Binary </h1>

```sh 
cd $HOME
rm -rf empowerchain

git clone https://github.com/EmpowerPlastic/empowerchain

cd empowerchain
cd chain

make install
```

<h1 align="center"> Genesis, addrbook ve servis </h1>

```sh
# bu satırı tek olarak kopyalayıp yapıştıralım.
curl -Ls https://ss-t.empower.nodestake.top/genesis.json > $HOME/.empowerchain/config/genesis.json 

# bu satırı tek olarak kopyalayıp yapıştıralım.
curl -Ls https://ss-t.empower.nodestake.top/addrbook.json > $HOME/.empowerchain/config/addrbook.json

# Servis dosyası:
sudo tee /etc/systemd/system/empowerd.service > /dev/null <<EOF
[Unit]
Description=empowerd Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which empowerd) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

# Resetleyelim
sudo systemctl daemon-reload
sudo systemctl enable empowerd
sudo systemctl restart empowerd
journalctl -u empowerd -f
```

<h1 align="center"> Cüzdan oluşturma </h1>

```sh
empowerd keys add "cüzdan_ismi"
# eğer daha önce cüzdan oluşturmuşsanız onu eklemek için 
empowerd keys add wallet --recover
```
<h1 align="center"> Validatör oluşturma </h1>

```sh
empowerd tx staking create-validator \
--amount=1000000umpwr \
--pubkey=$(empowerd tendermint show-validator) \
--moniker="Moniker" \
--identity=İdentity \
--details="details" \
--chain-id=circulus-1 \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-prices=0.1umpwr \
--gas-adjustment=1.5 \
--gas=auto \
-y 
```

<h1 align="center"> Node Silme  </h1>
# bu komut dizini node tamamen sunucudan siler silmeden önce yedek almayı unutmayın.

```sh
sudo systemctl stop empowerd && sudo systemctl disable empowerd && sudo rm /etc/systemd/system/empowerd.service && sudo systemctl daemon-reload && rm -rf $HOME/.empowerchain  && sudo rm -rf $(which empowerd) 
```
