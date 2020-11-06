# Pangea Docker container with CHIPS and LN inside the container

---------------------
## Run all inside the container

> **Be aware that Docker has EVERYTHING already installed, so you are not installing anything
You are only running things.**

Join the [CHIPS discord](https://discord.gg/bcSpzWb) to get a small amount of CHIPS

1. ### Run Docker container

    The Docker container will run as a daemon.

    `docker run --net=host --name bet -dit piggydoughnut/bet:v1.5`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. ### Run LN and Chips

    Enter the Docker container terminal

    `sudo docker exec -it bet bash`

    Run LN and CHIPS

    `./root/bet/scripts/runservices.sh`

    The output will look like this


    ```shell
    This will take about 3-5 minutes. Please be patient.

    Creating CHIPS config
    nonce.9250234
    0000006e75f6aa0efdbf7db03132aa4e4d0c84951537a6f5a7c39a0a9d30e1e7 <- genesis
    9bd1c477af8993947cdd9052c0e4c287fda95987b3cc8934b3769d7503852715 <- merkle
    Chips server starting
    |................................................................................| 100 % finished
    Starting Lightning node
    |................................................................................| 100 % finished

    =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    CHIPS and Lightning Node are running

    =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    NEXT STEPS

    1. FUND YOUR CHIPS ADDRESS with CHIPS to be able to join the game.
    { "address": "bUBbFW6yYTH3qXmynTyBtQ1FEcVSDT7ZNw" }

    2. Run bet (player OR dealer).

    DEALER
    cd ~/bet/privatebet && ./bet dcv 45.77.139.155

    PLAYER
    cd ~/bet/privatebet && ./bet player
    ```

3. ### Fund your CHIPS address. 

    Your address was shown to you in the output of the above command.

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

   

