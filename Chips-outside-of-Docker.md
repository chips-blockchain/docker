# Pangea Docker container with plugging in local CHIPS and LN

If you do not have the `ln` and `chips` running locally yet but would like to have that setup, see

## 1. Installing ln and chips locally

### Dependencies

```
$ sudo apt-get update
$ sudo apt-get install software-properties-common autoconf git build-essential libtool libprotobuf-c-dev libgmp-dev libsqlite3-dev python python3 zip libevent-dev pkg-config libssl-dev libcurl4-gnutls-dev make libboost-all-dev automake jq wget ninja-build libsqlite3-dev libgmp3-dev valgrind libcli-dev libsecp256k1-dev libsodium-dev libbase58-dev nano tmux

# Install Berkeley 4.8 db libs (chips dependecy)
# Source: https://cryptoandcoffee.com/mining-gems/install-berkeley-4-8-db-libs-on-ubuntu-16-04/
$ mkdir ~/db-4.8.30 && cd ~/db-4.8.30 && wget http://download.oracle.com/berkeley-db/db-4.8.30.zip && unzip db-4.8.30.zip
$ cd db-4.8.30 && cd build_unix/ && ../dist/configure --prefix=/usr/local --enable-cxx && make && make install
```


### Installing CHIPS
```
$ cd ~ && git clone https://github.com/chips-blockchain/chips && cd chips && ./build.sh
$ cd ~/chips/src && sudo cp chips-cli /usr/bin
$ sudo ldconfig /usr/local/lib # thanks smaragda!

--------------------
# Bootstraping CHIPS
--------------------
$ mkdir ~/.chips && cd ~/.chips && wget http://bootstrap3rd.dexstats.info/CHIPS-bootstrap.tar.gz
$ tar xvzf CHIPS-bootstrap.tar.gz
$ rm CHIPS-bootstrap.tar.gz
```

### Running CHIPS Daemon

  #### Create `chips.conf` file

  Create chips.conf file with random username, password, txindex and daemon turned on:
    
  ```shell
  cd ~
  mkdir .chips
  nano .chips/chips.conf
  ```

  Add the following lines into your `chips.conf` file

  ```JSON
  server=1
  daemon=1
  txindex=1
  rpcuser=chipsuser
  rpcpassword=passworddrowssap
  addnode=159.69.23.29
  addnode=95.179.192.102
  addnode=149.56.29.163
  addnode=145.239.149.173
  addnode=178.63.53.110
  addnode=151.80.108.76
  addnode=185.137.233.199
  rpcbind=127.0.0.1
  rpcallowip=127.0.0.1
  ```

  #### Symlinking the binaries (already done in Docker container)
  ```shell
  sudo ln -sf /root/chips/src/chips-cli /usr/local/bin/chips-cli
  sudo ln -sf /root/chips/src/chipsd /usr/local/bin/chipsd
  sudo chmod +x /usr/local/bin/chips-cli
  sudo chmod +x /usr/local/bin/chipsd
  ```
  #### Run
  ```shell
  cd ~
  cd chips/src
  ./chipsd &
  ```
  
         
  If you see something like this, the CHIPS daemon is still starting, give it a minute and try again.
  ```shell
  chips-cli getinfo
  error code: -28
  error message:
  Loading block index...
  ```
      

  #### Check
  ```shell
  chips-cli getinfo
  ```

  #### Preview block download status
  ```
  cd ~
  cd .chips
  tail -f debug.log
  ```
  
  Once it starts syncing you will see something like this
  ```shell
  2020-10-31T00:27:04Z UpdateTip: new best=00000013f930811f3c9d76be9447649e1f293208b8588fc7dae369cfc26fb024 height=7201018 version=0x20000000 log2_work=75.981897 tx=7435894 date='2020-10-31T00:27:03Z' progress=1.000000 cache=0.9MiB(6110txo)
  2020-10-31T00:27:08Z UpdateTip: new best=00000021bb8dd11b3a07b9e098518678255cf81fc8788fde018c3045e946f50a height=7201019 version=0x20000000 log2_work=75.981897 tx=7435895 date='2020-10-31T00:27:08Z' progress=1.000000 cache=0.9MiB(6111txo)
  2020-10-31T00:27:08Z AddToWallet d503a920f1070ecfbbb075f2c5f44f61b43b0fe8edea3a5287d95ef4efc782d3  new
  2020-10-31T00:27:26Z UpdateTip: new best=0000004729a5696107b0e3105f48e5270b7678120984e5f0314885531142fac3 height=7201020 version=0x20000000 log2_work=75.981897 tx=7435896 date='2020-10-31T00:27:26Z' progress=1.000000 cache=0.9MiB(6112txo)
  2020-10-31T00:27:26Z AddToWallet d4892b77f8b03d9606c343f4c465af4bc40cfd9e65b56af80d51779f6680fe6a  new
  ```

  If you see the following, its okay, dont mind it.
  ```shell
  2020-10-31T00:23:37Z connect() to 145.239.149.173:57777 failed after select(): Connection refused (111)
  2020-10-31T00:23:38Z connect() to 178.63.53.110:57777 failed after select(): Connection refused (111)
  2020-10-31T00:23:38Z connect() to 151.80.108.76:57777 failed after select(): Connection refused (111)
  ```

### Installing Lightning Network Node

```
$ cd ~ && git clone https://github.com/chips-blockchain/lightning.git
$ cd lightning && make
$ cd /usr/bin/ && sudo nano lightning-cli
# Insert the following into the newly created file

#/bin/bash
~/lightning/cli/lightning-cli $1 $2 $3 $4 $5 $6 | jq .

# ctrl + O to save the output and ctrl + X to exit nano editor

$ chmod +x /usr/bin/lightning-cli
```

### Running the Lightning Daemon

LN will need a while to sync. It could take some time so its a good idea to run it in a tmux session.

```
# Create a tmux session
$ tmux new -s lightning

# Then inside the tmux session you've just created
$ ~/lightning/lightningd/lightningd --log-level=debug &
# CTRL + B, then D to detach from the tmux session, to attach to the session again `tmux a -t lightning`

# Get chain info - If it returns your node’s id, you’re all set.
$ lightning-cli getinfo

# Get a new address to fund your Lightning Node
# This returns an address, which needs to be funded first in order to open a channel with another node.
$ lightning-cli newaddr

# Run the following command to check if your node has funds
$ lightning-cli listfunds

# Optionally, using these two parameters, you can connect to a node visible on the LN explorer
$ lightning-cli connect
$ lightning-cli fundchannel
```

Join the [CHIPS discord](https://discord.gg/bcSpzWb) to get a small amount of CHIPS

[Tmux cheatsheet](https://tmuxcheatsheet.com/)


## 2. Run Docker container

    `docker run --net=host --name bet -t -i -v /$HOME/.chips:/root/.chips:rw  -v /$HOME/.chipsln:/root/.chipsln:rw piggydoughnut/bet:v1.3`

## 3. Run bet

    [Run dealer](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-dealer)

    [Run player](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-player)    

    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

<img src="https://media.giphy.com/media/jQWUkD7a4AWfkraBJa/giphy.gif" width="300" height="262" />
