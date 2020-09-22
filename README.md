# Setup documentation for CHIPS Poker 

WORK IN PROGRESS: 

- Updating instructions inside of the `bet` repo

- Updating/testing information on https://docs.chips.cash/en/latest/

- Testing the https://github.com/NOCTLJRNE/CHIPS-tuto/blob/master/README.md

_______________

[About](README.md#about)

[Installation](README.md#installation)
    
- [Manual Installation](README.md#manual-installation-work-in-progress)
- [Docker](README.md#docker) 

[Compile the Docker image](README.md#compile-the-docker-image)

[More information on the project](README.md#more-information-on-the-project)

[Thank you](README.md#thank-you)

_______________

## About

[CHIPS FAQ](https://docs.chips.cash/en/latest/)

## Installation

At the current stage of the project there are a few options of how to install the game.

> The backend node on which any of the entities of the game are running must possess an IP address which is reachable over the internet. The condition to have a public ip address is mentioned below.

### Manual Installation (Work in progress)

> Note: Make sure to follow Install CHIPS-cli wallet first, the CHIPS daemon should be synced!

[Compilation guide](https://github.com/chips-blockchain/bet/blob/master/compile.md)

### Docker

[Docker image lives here](https://hub.docker.com/r/piggydoughnut/bet)

The Docker image has all the necessary libraries, dependencies and all have been installed, compiled, built, cloned, etc. You only need to start the processes needed for the game to work and do some configuration.

Everything lives in the home folder `cd ~`

You can:

- run the Docker container and run `chips` and `ln` inside of it  
OR
- plug in your `ln` and `chips` that will run outside of the container

Pull the image
    
    docker pull piggydoughnut/bet:v1.3

---------------------
#### Run all inside the container


1. Run Docker container

    `docker run --net=host --name bet -t -i piggydoughnut/bet:v1.3`

    > Note: the Docker container will use your VPS's host network so your player IP will be your VPS IP

2. Run Chips

    > Note: `rpcuser` and `rpcpassword` values can be anything you want

    - Follow [the repo instructions](https://github.com/chips-blockchain/chips#step-2-create-chips-data-dir-chipsconf-file-and-restrict-access-to-it)

3. Run the lightning node
    
    ```
    # Create a tmux session
    tmux new -s lightning
    # Then inside the tmux session you've just created
    ~/lightning/lightningd/lightningd --log-level=debug &
    # CTRL + B, then D to detach from the tmux session.
    ```

4. Run bet

    Depending on which part of the game you want to run see [the repo instructions](https://github.com/chips-blockchain/bet#configuring-the-table)

    If you simply want to play, you need to run the player node and specify the `DCV` IP, the dealer who runs the table you want to join.

        cd

        cd bet/privatebet

        ./bet player <DCV_IP>


    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.


---------------------
#### Plug in your `ln` and `chips` to the container

Your lightning network must be synced. If it is not, allow it to sync.

1. Run Docker container

    `docker run --net=host --name bet -t -i -v /$HOME/.chips:/root/.chips:rw  -v /$HOME/.chipsln:/root/.chipsln:rw piggydoughnut/bet:v1.3`

2. Run bet

    Depending on which part of the game you want to run see [the repo instructions](https://github.com/chips-blockchain/bet#configuring-the-table)

    If you simply want to play, you need to run the player node and specify the `DCV` IP, the dealer who runs the table you want to join.

        cd

        cd bet/privatebet

        ./bet player <DCV_IP>


    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

<img src="https://media.giphy.com/media/jQWUkD7a4AWfkraBJa/giphy.gif" width="300" height="262" />

## Compile the Docker image

If you want to collaborate on the Dockerfile or feel the desire to recompile the image yourself.

    docker build .

You need the `image-id` of the built image

    docker images

Run the image

    docker run --net=host --name bet -dit image-id


## More information on the project

[CHIPS documentation](https://docs.chips.cash/en/latest/)

## Thank you

This guide uses information from the following repositories. Thank you very much for all the provided information.

- https://github.com/chips-blockchain/chips_docs

- https://github.com/NOCTLJRNE/CHIPS-tuto/blob/master/README.md

- https://github.com/chips-blockchain/bet

- [JCharming](https://github.com/Jcharming) for all the help testing the instructions