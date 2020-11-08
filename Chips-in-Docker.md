# Pangea Docker container with CHIPS and LN inside the container


```diff
- LN might take several hours to sync. You might want to let it run overnight.
- Be aware that Docker has EVERYTHING already installed, so you are not installing anything. You are only running things.
```

The following is installation guide will work for Linux, Mac and Windows, providing that Docker is installed.

Each entity of the game (dealer, player) needs a separate IP address and a separate container to be run.
E.g. I want to run a player node and a dealer node. I need:
- 2 servers, each having a publicly accessible IP
- run Docker container on each server
- run dealer node inside of the first container
- run player node inside of the second container

1. ## Pull and run the docker image

    `docker pull piggydoughnut/bet:v1.5`

    The Docker container will run as a daemon.

    `docker run --net=host --name bet -dit piggydoughnut/bet:v1.5`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. ## Run LN and Chips

    Enter the Docker container terminal

    `sudo docker exec -it bet bash`

    Run LN and CHIPS. The following command will also generate a CHIPS address for you. It will show your address in the output.

    `./root/bet/scripts/runservices.sh`

3. ## Fund your CHIPS address. 

    Your address was shown to you in the output of the above command.
    
    Join the [CHIPS discord](https://discord.gg/bcSpzWb) to get a small amount of CHIPS

4. ## Run bet
    
    Run the node that you need to run. Are you hosting a game? -> Run Dealer. Do you just want to play the game and someone is hosting it for you? -> Run Player.

   ### Running Bet Dealer
    ```
    # e.g. Dealer node is at 45.77.139.155 (you will know this IP from someone who will be running a dealer node OR you can run the dealer node yourself, then you will use the IP of our dealer node)
    $ cd ~/bet/privatebet && ./bet dcv 45.77.139.155
    ```
   ### Running Bet Player
    ```
    $ cd ~/bet/privatebet && ./bet player
    ```

    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks. E.g.
    
    `ln is 286576 blocks behind chips network`
    
    Let the LN sync and come back later when ln has synched and run it again. You can exit the bet script (`Ctrl+C`), but DO NOT STOP the Docker container. Make sure it is running in the background, after you exit it. To exit the Docker container just type `exit` followed by an Enter. 
   

