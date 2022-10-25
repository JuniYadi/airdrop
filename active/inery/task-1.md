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
