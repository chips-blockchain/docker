# Pangea Docker container without plugging in local CHIPS and LN

---------------------
### Run all inside the container

> **Be aware that Docker has EVERYTHING already installed, so you are not installing anything
You are only running things.**

1. Run Docker container

    `docker run --net=host --name bet -t -i piggydoughnut/bet:v1.3`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. [Run Chips](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-chips-daemon)


3. [Run the lightning node](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-the-lightning-daemon)

    LN will need a while to sync. It could take some time so its a good idea to run it in a tmux session.

4. Run bet

    [Run dealer](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-dealer)

    [Run player](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-player)    

    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

