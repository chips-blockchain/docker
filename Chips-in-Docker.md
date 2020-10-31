# Pangea Docker container with CHIPS and LN inside the container

---------------------
## Run all inside the container

> **Be aware that Docker has EVERYTHING already installed, so you are not installing anything
You are only running things.**

1. ### Run Docker container

    `docker run --net=host --name bet -t -i piggydoughnut/bet:v1.4`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. ### Running CHIPS
    
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

      #### Run
      ```shell
      cd ~
      cd chips/src
      ./chipsd &
      ```

      #### Check
      ```shell
      chips-cli getinfo
      ```
       
      If you see something like this, the CHIPS daemon is still starting, give it a minute and try again.
      ```shell
      chips-cli getinfo
      error code: -28
      error message:
      Loading block index...
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

3. ### Run the lightning node

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

4. ### Run bet
    
    Run the node that you need to run. Are you hosting a game? -> Run Dealer. Do you just want to play the game and someone is hosting it for you? -> Run Player.
    
    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

   ### Running Bet Dealer
    ```
    # e.g. Dealer node is at 45.77.139.155 (you will know this IP from someone who will be running a dealer node OR you can run the dealer node yourself)
    $ cd ~/bet/privatebet && ./bet dcv 45.77.139.155
    ```
   ### Running Bet Player
    ```
    $ cd ~/bet/privatebet && ./bet player
    ```

   

