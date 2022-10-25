# NuLink

### Reference

* [https://blokdrops.medium.com/nulink-testnet-guide-earn-upto-6000-nlks-3a66d4f47407](https://blokdrops.medium.com/nulink-testnet-guide-earn-upto-6000-nlks-3a66d4f47407)
* [https://docs.nulink.org/products/nulink\_worker/eth\_account](https://docs.nulink.org/products/nulink\_worker/eth\_account)

### Claim Token Metamask

* [https://test-staking.nulink.org/](https://test-staking.nulink.org/)

### Install

{% code lineNumbers="true" %}
```
sudo su
cd $home
apt update && apt upgrade -y
apt install python3-pip -y
```
{% endcode %}

Generate Wallet

```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz
tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz
cd geth-linux-amd64-1.10.23-d901d853
./geth account new — keystore ./keystore
```

Masukkan password buat wallet, misalnya: **mYW4ll3tP445** terus copy result nya, contoh:

{% code overflow="wrap" lineNumbers="true" %}
```
Your new key was generated

Public address of the key:   0x630Fdd6229C0ee6B1546194E1Ac5fF81Df8c7004
Path of the secret key file: /root/.ethereum/keystore/UTC--2022-10-21T15-32-31.045522201Z--630fdd6229c0ee6b1546194e1ac5ff81df8c7004
```
{% endcode %}

Sekarang setup docker untuk jalanin neulink

```
cd $home
snap install docker
sleep 30
docker pull nulink/nulink:latest
mkdir -p /root/nulink
```

Disini kita mau copy file dari akun tadi, ganti sesuai akun kalian

<pre data-overflow="wrap" data-line-numbers><code><strong>cp /root/.ethereum/keystore/UTC--2022-10-21T15-32-31.045522201Z--630fdd6229c0ee6b1546194e1ac5ff81df8c7004 /root/nulink
</strong><strong>chmod -R 777 /root/nulink</strong></code></pre>

Disini kita akan siapin password neulink, password nya sama dengan password wallet

```
export NULINK_KEYSTORE_PASSWORD=mYW4ll3tP445
export NULINK_OPERATOR_ETH_PASSWORD=mYW4ll3tP445
```

Terus jalanin dockernya dengan command ini

```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/xxxxxxxxxxxxxxx \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address yyyyyyyyyyyyyyyy \
--max-gas-price 100
```

Ganti beberapa data berikut dengan Wallet Address Kalian

* xxxxxxxxxxxxxxx = UTC--2022-10-21T15-32-31.045522201Z--630fdd6229c0ee6b1546194e1ac5ff81df8c7004
* yyyyyyyyyyyyyyyy = 0x630Fdd6229C0ee6B1546194E1Ac5fF81Df8c7004

Sample Output

```
# step 1
 Detected IPv4 address (8.219.186.125) - Is this the public-facing address of Ursula? [y/N]: y
 
 Please provide a password to lock Operator keys.
 Do not forget this password, and ideally store it using a password manager.
 
 # step 2
 Enter nulink keystore password (8 character minimum): xxxxxx
 Repeat for confirmation: xxxxxx
 
 Backup your seed words, you will not be able to view them again.
 
 xxxxxxxxxxxxxxxxxxxxxxxx
 
 # step 3
 Have you backed up your seed phrase? [y/N]: y
 
 # step 4
 Confirm seed words: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
 
 
Public Key:   xxxxxxxxx
Path to Keystore: /home/circleci/.local/share/nulink/keystore

- You can share your public key with anyone. Others need it to interact with you.
- Never share secret keys with anyone! 
- Backup your keystore! Character keys are required to interact with the protocol!
- Remember your password! Without the password, it's impossible to decrypt the key!


Generated configuration file at default filepath /home/circleci/.local/share/nulink/ursula.json

* Review configuration  -> nulink ursula config
* Start working         -> nulink ursula run
```

Lanjut sekarang running nulink di background

```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

Cek Logs

```
docker logs -f ursula
```

Sample output

```
Authenticating Ursula
Loaded Ursula (horus)
✓ External IP matches configuration
Starting services
✓ Node Discovery (Horus)
✓ Work Tracking
✓ Start Operator Bonded Tracker
✓ Rest Server https://8.219.186.125:9151
Working ~ Keep Ursula Online!
```
