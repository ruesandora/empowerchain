<h1 align="center"> Empower Chain </h1>

![image](https://user-images.githubusercontent.com/101149671/193675081-0d205f2c-5425-40b4-80fb-b0920fabc74f.png)

## Discord kanal [linki](https://discord.gg/AN7uEyUb)

## Sistem gereksinimleri klasik cosmos:
```
4 CPU
8 RAM
200 SSD
```

# Güncellemeler:

```
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

## Go kurulumu:

```
cd $HOME && version="1.18.3" && \
wget "https://golang.org/dl/go$version.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$version.linux-amd64.tar.gz" && \
rm "go$version.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Binary:

```
cd $HOME && git clone https://github.com/empowerchain/empowerchain && \
cd empowerchain/chain && \
make install && \
empowerd version --long | head
```

## Moniker kısmını validator isminizi yazın.

```
empowerd init <MONIKER> --chain-id altruistic-1 && \
empowerd config chain-id altruistic-1
```

## Cüzdan oluşturun:

```
empowerd keys add <WALLET_NAME>
```

## Genesis oluşturuyoruz:

* <WALLET_NAME> kısmını düzenleyin.

```
empowerd add-genesis-account <WALLET_NAME> 1000000umpwr
```

## Gentx oluşturuyoruz:

* <WALLET_NAME> kısmını düzenleyin.

```
empowerd gentx <WALLET_NAME> 1000000umpwr \
--chain-id=altruistic-1 \
--moniker="<MONIKER>" \
--commission-max-change-rate 0.1 \
--commission-max-rate 0.2 \
--commission-rate 0.05 \
--pubkey $(empowerd tendermint show-validator) \
--website="" \
--security-contact="" \
--identity="" \
--details=""
```

## Gentx dosyanız oluştu:

* Winscp ile içersine girip: `/root/.empowerchain/config/gentx/` kısmından gentx'i masa üstünüze atın.
* Gentx isminizi şu şekilde düzenleyin, örnek: `gentx-RuesValidator.json`
* [Burayı](https://github.com/empowerchain/empowerchain) forklayın, not: bizim Buray'ı değil :)
* Kendi forkunuzun reposunda testnet klasörüne giriniz ve add file diyin:

![image](https://user-images.githubusercontent.com/101149671/193679961-7ff66bd6-ff22-4496-8389-745048bdeeee.png)

* Dosya ismini oluşturun ve gentx dosyasını içine atın ve kaydetin: 

![image](https://user-images.githubusercontent.com/101149671/193680137-52557ceb-e999-423c-b901-88c385709897.png)

* Sonra aynı repodan sol üstten pull request (PR) yapın.
* PR'ın içine atmanız gerek şu, masa üstüne attığınız dosyaya sağ tık yapıp not defteri ile açın, not defretrinden kopyalayıp PC'ın içine yapıştırın:

![image](https://user-images.githubusercontent.com/101149671/193680659-753d07ef-f8f4-4707-add5-92068f6d38d1.png)

![image](https://user-images.githubusercontent.com/101149671/193680773-eb1a12ee-303b-4132-af72-8679da6a5dd1.png)


## Genesisi indiriyoruz:

```
rm -rf $HOME/.empowerchain/config/genesis.json && cd $HOME/.empowerchain/config && wget https://raw.githubusercontent.com/empowerchain/empowerchain/main/testnets/altruistic-1/genesis.json
```

```
empowerd tendermint unsafe-reset-all --home $HOME/.empowerchain
```

## Kontrol:

```
sha256sum $HOME/.empowerchain/config/genesis.json
```

* Örnek çıktı: Result: fcae4a283488be14181fdc55f46705d9e11a32f8e3e8e25da5374914915d5ca8


## Servis dosyası oluşturuyoruz:
```
sudo tee /etc/systemd/system/empowerd.service > /dev/null <<EOF
[Unit]
Description=EmpowerChain Node
After=network.target
[Service]
User=$USER
Type=simple
ExecStart=$(which empowerd) start
Restart=on-failure
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

## Peerleri ekliyoruz:

```
seeds=""
peers="9de92b545638f6baaa7d6d5109a1f7148f093db3@65.108.77.106:26656,4fd5e497563b2e09cfe6f857fb35bdae76c12582@65.108.206.56:26656,fe32c17373fbaa36d9fd86bc1146bfa125bb4f58@5.9.147.185:26656,220fb60b083bc4d443ce2a7a5363f4813dd4aef4@116.202.236.115:26656,225ad85c594d03942a026b90f4dab43f90230ea0@88.99.3.158:26656,2a2932e780a681ddf980594f7eacf5a33081edaf@192.168.147.43:26656,333de3fc2eba7eead24e0c5f53d665662b2ba001@10.132.0.11:26656,4a38efbae54fd1357329bd583186a68ccd6d85f9@94.130.212.252:26656,52450b21f346a4cf76334374c9d8012b2867b842@167.172.246.201:26656,56d05d4ae0e1440ad7c68e52cc841c424d59badd@192.168.1.46:26656,6a675d4f66bfe049321c3861bcfd19bd09fefbde@195.3.223.204:26656,1069820cdd9f5332503166b60dc686703b2dccc5@138.201.141.76:26656,277ff448eec6ec7fa665f68bdb1c9cb1a52ff597@159.69.110.238:26656,3335c9458105cf65546db0fb51b66f751eeb4906@5.189.129.30:26656,bfb56f4cb8361c49a2ac107251f92c0ea5a1c251@192.168.1.177:26656,edc9aa0bbf1fcd7433fcc3650e3f50ab0becc0b5@65.21.170.3:26656,d582bcd8a8f0a20c551098571727726bc75bae74@213.239.217.52:26656,eb182533a12d75fbae1ec32ef1f8fc6b6dd06601@65.109.28.219:26656,b22f0708c6f393bf79acc0a6ca23643fe7d58391@65.21.91.50:26656,e8f6d75ab37bf4f08c018f306416df1e138fd21c@95.217.135.41:26656,ed83872f2781b2bdb282fc2fd790527bcb6ffe9f@192.168.3.17:26656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.empowerchain/config/config.toml
```


## Servisi başlatıyoruz:

```
sudo systemctl daemon-reload && \
sudo systemctl enable empowerd && \
sudo systemctl restart empowerd && \
sudo journalctl -u empowerd -f -o cat
```

<h1 align="center"> VALİDATOR OLUŞTURMA </h1>

 * Şu an faucet açılmadı 1-2 gün içersinde açılacak, o zaman duyuracağım:
 * Faucet açılıp token aldığınızda validatör oluşturun:

## Validator:

```
empowerd tx staking create-validator \
--amount=9900000umpwr \
--pubkey=$(empowerd tendermint show-validator) \
--moniker=RuesValidator \
--chain-id=altruistic-1 \
--commission-rate="0.10" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.1" \
--min-self-delegation="1" \
--fees=250umpwr \
--gas=200000 \
--from=rues \
--website="http://forum.rues.info/" \
--details="https://linktr.ee/ruesandora0" \
-y
```
