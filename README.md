<h1 align="center"> Empower Chain </h1>

<h1 align="center"> Testnete hazırlık </h1>

> Testnet yakında başlayacak, başlayana kadar sync olup hazırda bekletiyorum kendimi

> Testnet ve faucet açıldığında devamını paylaşacağım şimdilik sadece node kuruyoruz.

> Topluluk kanalları: [Duyuru](https://t.me/RuesAnnouncement) - [Sohbet](https://t.me/RuesChat) - [Empower Discord](https://discord.gg/Zs3GMUhg)

<h1 align="center"> Donanım </h1>

```sh
# Hetzner'den benim kurduğum:
2 CPU
4 RAM
20.04 Ubuntu version

# Dökümasyonda yazan
4 CPU
8 RAM
200 SSD
```

<h1 align="center"> Güncelleme </h1>

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

# go version 1.20 olmak zorunda yoksa hata alırsınız
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
# Orjinal dökümasyonda genesis URL'ler yok, manuel ekliyorum:
curl -Ls https://ss-t.empower.nodestake.top/genesis.json > $HOME/.empowerchain/config/genesis.json 

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
# Güncel blok 8000 küsür.
journalctl -u empowerd -f
```

<h1 align="center"> Cüzdan oluşturma </h1>

```sh
# rues'i değiştirin.
empowerd keys add rues
```
