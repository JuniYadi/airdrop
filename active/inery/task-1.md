# Task 1

### Install

#### Setup Requirment

```
sudo apt-get install -y make bzip2 automake libbz2-dev libssl-dev doxygen graphviz libgmp3-dev \
autotools-dev libicu-dev python2.7 python2.7-dev python3 python3-dev \
autoconf libtool curl zlib1g-dev sudo ruby libusb-1.0-0-dev \
libcurl4-gnutls-dev pkg-config patch llvm-7-dev clang-7 vim-common jq libncurses5
```

#### Clone Repository

```
git clone https://github.com/inery-blockchain/inery-node
cd inery-node
ls
cd inery.setup
chmod +x ine.py
```

#### Export Binary

```
./ine.py --export
cd; source .bashrc; cd -
```

#### Start Master Node

```
cd tools
nano config.json
```

#### Ganti Detail Ini Sesuai Akun Terdaftar di Inery

```
"MASTER_ACCOUNT":
{
    "NAME": "AccountName",
    "PUBLIC_KEY": "PublicKey",
    "PRIVATE_KEY": "PrivateKey",
    "PEER_ADDRESS": "IP:9010",
    "HTTP_ADDRESS": "0.0.0.0:8888",
    "HOST_ADDRESS": "0.0.0.0:9010"
}
```

#### Jalankan Program

```
cd ..
screen -S master
./ine.py --master
```

**Setup Wallet**

Buka tab baru terus konek ke vps, yang dari command diatas biarkan saja supaya running

```
cd; cline wallet create --file defaultWallet.txt
```

**Cek Wallet Password**

```
cat defaultWallet.txt;echo;
```

**Connect ke Wallet**

```
cline wallet unlock --password YOUR_WALLET_PASSWORD
cline wallet import --private-key MASTER_PRIVATE_KEY
cline system regproducer ACCOUNT_NAME ACCOUNT_PUBLIC_KEY 0.0.0.0:9010
cline system makeprod approve ACCOUNT_NAME ACCOUNT_NAME
```

**Catatan:**

* YOUR\_WALLET\_PASSWORD = cek di file defaultWallet.txt
* MASTER\_PRIVATE\_KEY = dari Privatekey di Web Inery
* ACCOUNT\_NAME = dari Web Inery
* ACCOUNT\_PUBLIC\_KEY = dari Web Inery
