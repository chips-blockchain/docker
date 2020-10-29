# Pangea Docker container with plugging in local CHIPS and LN

If you do not have the `ln` and `chips` running locally yet but would like to have that setup, see

[Dependencies check](https://github.com/chips-blockchain/bet/blob/master/compile.md#dependencies-check)

[Installing CHIPS](https://github.com/chips-blockchain/bet/blob/master/compile.md#installing-chips)

[Installing Lightning Network Node](https://github.com/chips-blockchain/bet/blob/master/compile.md#installing-lightning-network-node)

You can save yourself some time by only installing relevant dependencies (do not insall bet dependencies!). Bet dependencies are precompiled in the Docker container.

Your lightning node must be synced. If it is not, allow it to sync. 

1. Run Docker container

    `docker run --net=host --name bet -t -i -v /$HOME/.chips:/root/.chips:rw  -v /$HOME/.chipsln:/root/.chipsln:rw piggydoughnut/bet:v1.3`

2. Run bet

    [Run dealer](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-dealer)

    [Run player](https://github.com/chips-blockchain/bet/blob/master/compile.md#running-bet-player)    

    If you have not allowed for the ln to sync the script will tell you that it is behind ln by a number of blocks.
You can either leave the script running until it syncs or come back later when ln has synched and run it again.

<img src="https://media.giphy.com/media/jQWUkD7a4AWfkraBJa/giphy.gif" width="300" height="262" />