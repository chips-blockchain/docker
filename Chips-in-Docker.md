# Pangea Docker container with CHIPS and LN inside the container


```diff
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

    `docker pull piggydoughnut/bet:v1.6`

    The Docker container will run as a daemon.

    `docker run --net=host --name bet -dit piggydoughnut/bet:v1.6`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. ## Run Chips

    Enter the Docker container terminal

    `sudo docker exec -it bet bash`

   
   ### Run CHIPS daemon

    `chipsd &`

   #### Check if CHIPS is running

    `chips-cli getinfo`

    The output might be an error followed by Loading blocks.... CHIPS is loading our bootstrapped blocks. Just give it some time and try again later. 

   #### Wait for CHIPS to fully sync.

    Can take about 1-2 hours.

    CHIPS is fully synced when the number of blocks and headers match.

    ```
    chips-cli getinfo
    {
      ...
      ...
      "chain": "main",
      "blocks": 7709392,
      "headers": 7709392,
      ...
      ...
    } 
    ```
3. ## Run Lightning

   Run the lightning node in tmux

   Create a tmux session
   ```
   tmux new -s lightning
   ```

   Then inside the tmux session you've just created
   ```
   lightningd --log-level=debug &
   # CTRL + B, then D to detach from the tmux session, to attach to the session again `tmux a -t lightning`
   ```

   Get chain info - If it returns your node’s id, you’re all set.
   ```
   lightning-cli getinfo
   ```

   If you don't see output `lightning-cli` of this command formatted, you can add `jq` with pipe command like following:
   ```
   lightning-cli getinfo | jq .

4. ## Fund your nodes. 
    
    Once you setup your nodes, you need to fund them. See [Fund your nodes instructions](./setup_fund_nodes.md).
    
    Join the [CHIPS discord](https://discord.gg/bcSpzWb) to get a small amount of CHIPS

5. ## Run bet
    
   Run the node that you want to run. Are you hosting a game? -> Run Dealer. Do you just want to play the game and someone is hosting it for you? -> Run Player.

   Check out [Tutorial](https://github.com/chips-blockchain/pangea-poker/blob/dev/tutorial/Tutorial.md)

