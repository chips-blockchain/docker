# Pangea Docker container without plugging in local CHIPS and LN

---------------------
## Run all inside the container

> **Be aware that Docker has EVERYTHING already installed, so you are not installing anything
You are only running things.**

1. ### Run Docker container

    `docker run --net=host --name bet -t -i piggydoughnut/bet:v1.3`

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

  #### Preview block download status
  ```
  cd ~
  cd .chips
  tail -f debug.log
  ```


3. Run the lightning node

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

4. Run bet
    
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

   

