# Docker documentation for CHIPS Poker 

[Manual Installation](README.md#manual-installation)

[Docker](README.md#docker) 

[Compile the Docker image](README.md#compile-the-docker-image)

[FAQ](README.md#more-information-on-the-project)

[Thank you](README.md#thank-you)

> Any of the nodes which are used to play must have a publicly accessible IP.


## Manual Installation

[Compilation guide](https://github.com/chips-blockchain/bet/blob/master/compile.md)

## Docker

[Docker image lives here](https://hub.docker.com/r/piggydoughnut/bet)

Pull the image
    
    docker pull piggydoughnut/bet:v1.4

The Docker image has all the necessary libraries, dependencies and all have been installed, compiled, built, cloned, etc. You only need to start the processes needed for the game to work and do some configuration.

Everything lives in the home folder `cd ~`

You can:

- [Run the Docker container and run `chips` and `ln` inside of it](Chips-in-Docker.md)

  Be aware, if you delete your Docker container by accident you can loose the synced LN, your wallet, etc.

OR

- [Plug in your `ln` and `chips` that will run outside of the container](Chips-outside-of-Docker.md)

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
