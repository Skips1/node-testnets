# <h1 align="center">Massa testnet 15</h1>

![massa](https://user-images.githubusercontent.com/73015593/180018941-20b9e515-6e94-47ef-b928-b8e3ba507691.jpg)

# Linkler
> ## [Discord](https://discord.gg/J7scURTM)<br>
> ## [Massa website](https://massa.net/)
> ## [Massa explorer](https://paranormal-brothers.com/massa/)

# Sistem gereksinimleri
```
8GB RAM
50-100 GB SSD
4 vCPU
```

# Node kurulumu

## Sistem güncellemesi yapıyoruz.
```
sudo apt update && sudo apt upgrade -y
```
![image](https://user-images.githubusercontent.com/73015593/180020260-2774f980-3211-458b-afe3-230f8dc89a34.png)


## Kütüphane kurulumu yapıyoruz.
```
sudo apt install make clang curl pkg-config libssl-dev build-essential git jq ncdu bsdmainutils -y < "/dev/null"
```
![image](https://user-images.githubusercontent.com/73015593/180020703-faa62886-4328-45a2-9136-37d70a097f1c.png)

## Rust kurulumu yapıyoruz. 1 diyerek Proceed with installation (default) kurulumu yapıyoruz.
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```
![image](https://user-images.githubusercontent.com/73015593/180025712-25d76b2a-72f4-43ee-abf2-6bdb110359bc.png)

## Rust sürümünü kontrol edelim.
```
rustc --version
```
![image](https://user-images.githubusercontent.com/73015593/180025948-fa63daf2-5b2e-4de5-8e55-05b46f3a1cbc.png)

## nightly indiriyoruz.
```
rustup toolchain install nightly
```
![image](https://user-images.githubusercontent.com/73015593/180026786-c0cfc916-1f77-4834-9c20-08720564f10d.png)

## varsayılan olarak ayarlıyoruz.
```
rustup default nightly
```

## massa repository'den `massa 15.1` binary dosyasını indiriyoruz.
```
wget -O massa.tar.gz https://github.com/massalabs/massa/releases/download/TEST.15.1/massa_TEST.15.1_release_linux.tar.gz
tar -xzf massa.tar.gz
rm massa.tar.gz
```

## Servis dosyası oluşturuyoruz. `[PASSWORD]` kısmına cüzdan şifremizi giriyoruz.
```
echo "[Unit]
Description=Massa Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/massa/massa-node/
ExecStart=$HOME/massa/massa-node/massa-node -p [PASSWORD]
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/massa-node.service
sudo mv $HOME/massa-node.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```

## `HOME/massa/massa-node/base_config/config.toml` dosyamıza giriyoruz.
```
nano $HOME/massa/massa-node/base_config/config.toml
```

## bootstrap kısmını yeni bootstrap listesi ile değiştiriyoruz.
```
[bootstrap]
bootstrap_list = [
  ["116.202.91.170:31245", "P124V1FNLotWWbHSuHxgSrcsCFNbUEhAWbcXxaT9Z6p3kWNqMA5j"],
  ["86.233.104.63:31245", "P12C9dme5WAZ3VHNjzjM9vF3JAJyMSJJf17qscqk27ktDUGhPq9v"],
  ["135.181.82.43:31245", "P12DApGgeUz9f3C9eY4cyHGBPzJs1N8UnzDw2E2RcPHhsTbVHaFB"],
  ["149.102.157.146:31245", "P12KvWkgrcpFHU6F9UATLV4LgfCZYMNsHspAr7brSU32crYk2gkw"],
  ["149.202.86.103:31245", "P12UbyLJDS7zimGWf3LTHe8hYY67RdLke1iDRZqJbQQLHQSKPW8j"],
  ["78.197.201.178:31245", "P12V6DRSGgfKWcx7snZ8aDrkC8S1NMcXFVwhgo9UU8yJybaj7M73"],
  ["95.111.250.80:31245", "P12XqWRRdsEA7yPoCWBpZvgwMQfsmw8rQERmB5bZQmsXyKzvkufC"],
  ["195.201.194.188:31245", "P12YgBrrQCUL1Mbi3E3xrQCYtMNBidcDYzJawUgwa5hauCp4RR9T"],
  ["139.59.247.49:31245", "P12dY4M648QdZsR6fPamsNjwZgZwQYuVZ98WFuWLirmfhAqhamuj"],
  ["82.223.10.164:31245", "P12mqWgZdS3qYQcgYXD4PPtL128U7bfTSSsZRWHn4mQCcaYc93y3"],
  ["158.69.120.215:31245", "P12rPDBmpnpnbECeAKDjbmeR19dYjAUwyLzsa8wmYJnkXLCNF28E"],
  ["129.159.141.247:31245", "P12tfvwoLevhM39RxUC8SBPEvv5H97VPjoqKCcKHVtnep6VvhyYC"],
  ["149.202.89.125:31245", "P12vxrYTQzS5TRzxLfFNYxn6PyEsphKWkdqx2mVfEuvJ9sPF43uq"],
  ["51.75.60.228:31245", "P13Ykon8Zo73PTKMruLViMMtE2rEG646JQ4sCcee2DnopmVM3P5"],
  ["82.66.176.15:31245", "P16CbJQ5USfJ4YmoeuL5j3qjLRXWR6EyS1yLepBzZvxbJT2X3zb"],
  ["80.73.4.50:31245", "P18hxJUwQcCRWijcY4Vq2jgE6xexSV7FS1cJvKbxCkG4dm9Prni"],
  ["185.69.52.194:31245", "P19pFgS9c9wAoM59jo97TTeChGD7zDgE7mMmPPvJp9HX2VocUsd"],
  ["103.246.144.157:31245", "P1AX9MrVkK36QunfZ5yh7eD1J6d7ZPFfoJqqbXypybmsKS49gd6"],
  ["5.161.110.104:31245", "P1HBPGnkdvaKfAb7L24FDMGRPQsbaixY7S3PyK6Rx95wd8TvwsC"],
  ["78.61.234.194:31245", "P1Ly8ZznKLEV7GCth3EbdJxLdCEp4KrGRsBmbPveqFB1MAJzyrW"],
  ["138.124.184.213:31245", "P1PDGgm8Agn48VzGHm4MtJEkjVcY38V5ezL5DmKNNTroZrKXZcm"],
  ["54.38.34.170:31245", "P1U4qziyrjiVqUCPpgFtSSE5cuVWXvyGb9zfCKK38TcNVbKwb3r"],
  ["82.66.199.77:31245", "P1U72FiX55zFwzRRCCmnr2pyiXgEDagDLshLt7XHHvhxXZNbDvH"],
  ["158.69.23.120:31245", "P1XxexKa3XNzvmakNmPawqFrE9Z2NFhfq1AhvV1Qx4zXq5p1Bp9"],
  ["77.40.50.164:31245", "P1eNDEywNTJDByV7XQE9UG4o1x2JoY9YZTtqDNNUbhRHY9EVmvR"],
  ["54.36.174.177:31245", "P1gEdBVEbRFbBxBtrjcTDDK9JPbJFDay27uiJRE3vmbFAFDKNh7"],
  ["198.27.74.52:31245", "P1hdgsVsd4zkNp8cF1rdqqG6JPRQasAmx12QgJaJHBHFU1fRHEH"],
  ["49.12.235.159:31245", "P1hiyVxJTSno8aLyTNvtaU4Tqux64xJB2ruV6KiMYUaR2EG3c8u"],
  ["82.146.54.109:31245", "P1hwV1zpR9ne1Ykoqa1kTaTpwKu7NCzGZSQLJZ1XeBkshUhHtjW"],
  ["185.8.156.122:31245", "P1qx14Arb54ZBXrdt8Nes7evqn7dfa14XJmAqSHCLacDJqBtML4"],
  ["198.27.74.5:31245", "P1qxuqNnx9kyAMYxUfsYiv2gQd5viiBX126SzzexEdbbWd2vQKu"],
  ["135.181.83.64:31245", "P1s9ju13MZmpnS6AvwaK3fWwqaDoMdijCcoME4dQUFiUHfp6cea"],
  ["185.202.223.167:31245", "P1yLjYoaL7TmzErZHLXqotv5Pj3DwRoeX7oCbSbVtWVbJPLbTuG"]
]
```
![image](https://user-images.githubusercontent.com/73015593/189427058-9bea2587-c585-4724-9ee2-c554cbd622d9.png)

## Servisimizi başlatıyoruz.
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable massa-node
sudo systemctl restart massa-node
```

## Daha önce wallet oluşturanlar eski wallet.dat dosyasını sunucuda `$HOME/massa/massa-client` dizini altına atıyoruz.
![image](https://user-images.githubusercontent.com/73015593/189427917-632b6ff0-cd0b-45ed-9181-7c8080973d1e.png)

## massa client başlatıyoruz. 
* `[walletpassword]` kısmına cüzdan şifremizi giriyoruz.
* --wallet $HOME/massa/massa-client/wallet.dat yerine daha önce cüzdan oluşturmadıysanız `wallet_generate_secret_key` girin.
```
cd $HOME/massa/massa-client/
./massa-client --wallet $HOME/massa/massa-client/wallet.dat -p [walletpassword]
```
![image](https://user-images.githubusercontent.com/73015593/189431398-7ad587a0-c9b1-46b2-b485-ee370ccf4c5a.png)

## [Massa Discord](https://discord.gg/J7scURTM)'una gidiyoruz. 
![image](https://user-images.githubusercontent.com/73015593/189432317-2044f944-e9ac-4f72-bf62-3d8a0911289a.png)

## #⌠✅⌡testnet-rewards-registration kanalına gidiyoruz ve rastgele bir mesaj atıyoruz. Ardından bot bize özelden mesaj atacak.
![image](https://user-images.githubusercontent.com/73015593/189432602-d46fae1f-01fb-4f06-9080-5f5ea50c7940.png)

## #⌠💸⌡testnet-faucet kanalına gidiyoruz ve cüzdan adresimizi gönderiyoruz. (Cüzdan bilgilerine bakmak için `wallet_info` komutunu gireriz.)
![image](https://user-images.githubusercontent.com/73015593/189433642-d563dec7-ef95-4044-b951-11274241c359.png)

## Gelen tokenler ile rol satın alıyoruz. (`walletAddress` kısmına cüzdan adresimizi giriyoruz.)
```
buy_rolls walletAddress 1 0
```
![image](https://user-images.githubusercontent.com/73015593/189447804-5c93be9d-eb6f-4eed-9a59-bf5963518aea.png)

## Bot'a sunucu ip adresimizi gönderiyoruz.
![image](https://user-images.githubusercontent.com/73015593/189433133-d69be7c4-0895-4e1d-83f7-d6888c7784a3.png)

## secret key'imizi kaydediyoruz. `secretKeys` kısmına wallet secret key'imizi giriyoruz. (Cüzdan bilgilerine bakmak için `wallet_info` komutu gireriz.) 
```
node_add_staking_secret_keys secretKeys 
```

## Testnete kayıt yaptırıyoruz. (`staking_address` kısmına cüzdan adresimizi, `discordId` kısmına discord Id mizi giriyoruz.) 
```
node_testnet_rewards_program_ownership_proof staking_address discordId
```
![image](https://user-images.githubusercontent.com/73015593/189438905-871ec8b4-6ef9-43cd-8245-b9cb747c6a78.png)

## Oluşan çıktıyı bota gönderiyoruz.
![image](https://user-images.githubusercontent.com/73015593/189439038-0189b397-dadb-42f6-aa0a-36bd40c2c826.png)

## Tüm her şeyi bot'a `info` yazarak kontrol edebiliriz.
![image](https://user-images.githubusercontent.com/73015593/189439183-a69e2f3b-de4c-4c05-9b0b-180639b4a86a.png)


# Script ile kurmak isteyenler için [LetskyNode](https://teletype.in/@letskynode/Massa) hazırladığı harika bir döküman.










