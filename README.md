# Setup documentation for CHIPS Poker 

WORK IN PROGRESS:

- Testing out manual installation

- Updating instructions inside of the `bet` repo

- Updating/testing information on https://docs.chips.cash/en/latest/

- Testing the https://github.com/NOCTLJRNE/CHIPS-tuto/blob/master/README.md

## About

This is a high level technical overview of everything you need to know about the CHIPS project. Each section has links provided for a more in-depth study.

[CHIPS FAQ](https://docs.chips.cash/en/latest/)

### Terms

`CHIPS` - game token and the name of the poker project.

`Pangea` - protocol on which the poker game is based on

`DCV` - Deck Creating Vendor, the Dealer node

`BVV` - Blinding Value Vendor, the component that helps securelt shuffle the deck

`Cashier` or `Notary` nodes - The trusted nodes in the network and are elcted and chosen by the community. The set of trusted nodes at the moment are [here](https://github.com/chips-blockchain/bet/blob/master/privatebet/config/cashier_nodes.json).


### Preconditions

#### Public IP address

The backend node on which any of the entities of the game are running must possess an IP address which is reachable over the internet. The condition to have a public ip address is mentioned below.

1. The `Player`, `DCV` and `BVV` nodes connects to the backend via GUI, by entering the IP address in the GUI. (Temporary alpha version setup, while we are working on the dht protocol implementation)

2. All the entities in the game must connect to `DCV`.
    
     `Player` and `BVV` nodes which are part of the game are connected to `DCV` node using the network sockets in the backend. 

     For the connection to be established the Player node must specify `DCV` IP address when it starts. The player chooses which `DCV` he wants to connect to.

> Note: The best would be to have the game setup on a remote server so you can easily use the VPS IP instead of exposing your own IP to the internet, which could pose security risks.

#### Port Information

1. All the player nodes, DCV and BVV are connected in the backend on the ports `7797` and `7798`

    1. port `7797` is used to establish `pub-sub` relationship between the `DCV` and `{players, BVV}`.
    2. port `7798` is used to establish `pull-push` relationship between the `DCV` and `{players, BVV}`.
    
2. `Player` node needs to communicates with the `Cashier` node in order to lock and unlock the funds. `Cashier` node runs on the port `7901`.

3. `Player`, `DCV` and `BVV` nodes connects to the backend via GUI using the port `9000` by establishing the websocker connection.


## Installation

At the current stage of the project there are a few options of how to install the game.

### Manual Installation (Work in progress)

> Note: Make sure to follow Install CHIPS-cli wallet first, the CHIPS daemon should be synced!

1. [Install CHIPS-cli wallet](https://docs.chips.cash/en/latest/install-cli.html)

2. [Install Lightning Network node](https://docs.chips.cash/en/latest/install-ln.html)

3. [Install BET](https://docs.chips.cash/en/latest/install-bet.html)

### Docker

You can:

- run the Docker container and run `chips` and `ln` inside of it 
- plug in your `ln` and `chips` that will run outside of the container

Pull the image
    
    `docker pull piggydoughnut/bet:v1.3`

---------------------
#### Run all inside the container


1. Run Docker container

    `docker run --net=host --name bet -t -i piggydoughnut/bet:v1.3`

    note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. Enter the Docker container

    `docker exec -it bet bash`

3. [Run Chips](https://github.com/chips-blockchain/chips#step-2-create-chips-data-dir-chipsconf-file-and-restrict-access-to-it)

    - `rpcuser` and `rpcpassword` values can be anything you want

    - You could download chips blocks to speed up the sync if you want.


            Download and extract bootstrap files:

            cd ~/.chips
            wget http://bootstrap3rd.dexstats.info/CHIPS-bootstrap.tar.gz
            tar xvzf CHIPS-bootstrap.tar.gz


            Delete Bootstrap file once done to clear space

            cd ~/.chips 
            rm CHIPS-bootstrap.tar.gz


            Use df -H to get a readout of used diskspace if needed.

4. [Run the lightning node](https://docs.chips.cash/en/latest/install-ln.html#running-the-lightning-node)


5. [Run bet](https://github.com/chips-blockchain/bet)


    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

6. You are all done


---------------------
#### Plug in your `ln` and `chips` to the container

Your lightning network must be synced. If it is not, allow it to sync.

1. Run Docker container

    `docker run --net=host --name bet -t -i -v /$HOME/.chips:/root/.chips:rw  -v /$HOME/.chipsln:/root/.chipsln:rw piggydoughnut/bet:v1.3`

2. Enter the Docker container

    `docker exec -it bet bash`

3. [Run bet](https://github.com/chips-blockchain/bet)

### Compile the Docker image yourself

If you want to collaborate on the Dockerfile or feel the desire to recompile the image yourself.

    `docker build .`

You need the `image-id` of the built image

    `docker images`

Run the image

    `docker run --net=host --name bet -dit image-id`



## Infrastructure

CHIPS is a bitcoin fork. It uses lightening protocol for the micro transactions that happen in the game in real time. For more info see [What is CHIPS?](https://docs.chips.cash/en/latest/faq.html#what-is-chips)

### Message Communication

All the communication in the game must happen though `DCV`. Pangea Protocol does not allow any direct communication between the Players and the `BVV`. Players and `BVV` connect to `DCV` via `NN_PUSH/NN_PULL` socket. If any entity in the game is willing to send a message, it sends it to `DCV` via `NN_PUSH`, and `DCV` receives it via `NN_PULL`.

<img src="./assets/PULL.png" width="500">

Once the `DCV` receives the messages it publishes it via `NN_PUB` and since Players and `BVV` are subscribed to `DCV` via `NN_SUB` so whenever the `DCV` publishes messages the Players and `BVV` receive it.

<img src="./assets/Messages.png" width="500">

Source: Pangea Protocol Whitepaper (authors: [sg777](https://github.com/sg777), [jl777](https://github.com/jl777/))

If you want to know all the details please refer to the [Pangea Protocol Whitepaper](https://cdn.discordapp.com/attachments/455737840668770315/456036359870611457/Unsolicited_PANGEA_WP.pdf)

## More information on the project

[CHIPS documentation](https://docs.chips.cash/en/latest/)

## Thank you

This guide uses information from the following repositories. Thank you very much for all the provided information.

- https://github.com/Meshbits/chips_docs

- https://github.com/NOCTLJRNE/CHIPS-tuto/blob/master/README.md

- https://github.com/chips-blockchain/bet