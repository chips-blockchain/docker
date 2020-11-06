# Pangea Docker container with CHIPS and LN inside the container

---------------------
## Run all inside the container

> **Be aware that Docker has EVERYTHING already installed, so you are not installing anything
You are only running things.**


1. ### Pull and run the docker image

    `docker pull piggydoughnut/bet:v1.5`

    The Docker container will run as a daemon.

    `docker run --net=host --name bet -dit piggydoughnut/bet:v1.5`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. ### Run LN and Chips

    Enter the Docker container terminal

    `sudo docker exec -it bet bash`

    Run LN and CHIPS. The following command will also generate a CHIPS address for you. It will show your address in the output.

    `./root/bet/scripts/runservices.sh`

3. ### Fund your CHIPS address. 

    Your address was shown to you in the output of the above command.
    
    Join the [CHIPS discord](https://discord.gg/bcSpzWb) to get a small amount of CHIPS

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

   

