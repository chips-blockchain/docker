# Docker documentation for CHIPS Poker 


[Docker image on DockerHub](https://hub.docker.com/r/piggydoughnut/bet)

## Run Docker to play the game

The Docker image has all the necessary libraries, dependencies nstalled, compiled, built, cloned, etc. You only need to start the processes needed for the game to work and do some configuration.

Everything lives in the home folder `cd ~`

There are two options for two types of users.

### OPTION 1 

You have done no preparations and want the easiest option to run the game. 

[Run the Docker container and run `chips` and `ln` inside of it](Chips-in-Docker.md)

### OPTION 2 

You know what you are doing and already have `CHIPS` and `LN` running locally (or you would like to have). 

[Plug in your `ln` and `chips` that will run outside of the container](Chips-outside-of-Docker.md)


## More information on the project

https://chips.cash

## Thank you

This guide uses information from the following repositories. Thank you very much for all the provided information.

- https://github.com/chips-blockchain/chips_docs

- https://github.com/NOCTLJRNE/CHIPS-tuto/blob/master/README.md

- https://github.com/chips-blockchain/bet

- [JCharming](https://github.com/Jcharming) for all the help testing the instructions
