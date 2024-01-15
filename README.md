<h1 align="center">Massa Mainnet </h1>

# Selam, Massa Mainnet Node Kurulum rehberi:

## Sistem gereksinimleri:

Tavsite Edilen Sistem:
```
8 CPU
16 RAM
1 TB SSD
```
Denenmiş Yeterli  Slstem:
```
4 CPU
8 RAM
400 GB SSD
```

## Ubuntu'da bu kütüphanelerin kurulumu:
```
sudo apt install pkg-config curl git build-essential libssl-dev libclang-dev cmake
```

## Rustup'ı yükleyin :
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## yolu yapılandır:
```
rustc --version
```

## Rustup sürümü yükleyin:
```
rustup toolchain install 1.74.1
```

## varsayılan olarak ayarlayın: 
```
rustup default 1.74.1
```

## Sürüm kontrol:
```
rustc --version
```

## MASSA repoyu klonla:
```
git clone https://github.com/massalabs/massa.git
```

## Portları açın:
```
sudo apt install ufw 
sudo ufw default deny incoming 
sudo ufw default allow outgoing 
sudo ufw status
sudo ufw enable
ufw allow 22
ufw allow 22/tcp
ufw allow 31244
ufw allow 31244/tcp
ufw allow 31245
ufw allow 31245/tcp
sudo ufw status
```

## Config dosyası düzenleme: 
 * IP_ADDRESS  yerine vps ipnizi girin 
 * Hepsini kopyalaypp tek seferde girin
 
```
sudo tee <<EOF >/dev/null $HOME/massa/massa-node/config/config.toml 
[protocol] 
routable_ip = "IP_ADDRESS"
EOF
```
## SERVİCE DOSYASI OLUŞTURMA:
 *-p den sonra şifrenizi yazın XXXX olan yere
 * Hepsini kopyalaypp tek seferde girin
 
```
printf "[Unit]
Description=Massa Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/massa/massa-node
ExecStart=$HOME/.cargo/bin/cargo run --release -- -p XXXX
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target" > /etc/systemd/system/massad.service
```

## Sevice Aktif Etme
```
sudo systemctl daemon-reload
sudo systemctl enable massad
sudo systemctl restart massad
```
## node restart:
```
sudo systemctl restart massad
```
## node stop:
```
sudo systemctl stop massad
```
## Log kontrol:
```
sudo journalctl -f -n 100 -u massad
```
## Client giriş:
 * Cüzdan import
 * Stake
 * Roll alma satma işlemleri:
 *  *-p den sonra şifrenizi yazın XXXX olan yere
 *  İşlemlerin hepsini Command satırında yapacağıc
 *  Çıkış CTRL+C
```
cd massa/massa-client/
cargo run --release -- -p XXXX
```   
## Massa Cüzdan import
 * Massalarınızın bulunduğu cüzdanın secret keyi
```
wallet_add_secret_keys XXXXXXXXXXXXXXXXXXXXXXXXX
```
## Massa Stake
 * Massalarınızın bulunduğu cüzdanın adresi
 * Komut Çıktısı "Keys successfully added!"
```
node_start_staking XXXXXXXXXXXXXXXXXXXXXXXXX
```
* Stake adres kontrol:
```
node_get_staking_addresses
```
 * Roll alma işlemleri: X:Cüzdan adresi  10:rolsayısı (1rol=100massa) 0:Fee varsayılan 
```
buy_rolls XXXXXXXXXXXXXXXXXXXXXXXXX 10 0
```
* Roll satma işlemleri: X:Cüzdan adresi  10:rolsayısı (1rol=100massa) 0:Fee varsayılan 
```
sell_rolls XXXXXXXXXXXXXXXXXXXXXXXXX 10 0
```
## Node Kontrol:
```
get_addresses XXXXXXXXXXXXXXXXXXXXXXXXX
```
## Cüzdan Kontrol:
```
wallet_info
```
