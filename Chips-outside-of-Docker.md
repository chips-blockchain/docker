# Pangea Docker container with plugging in local CHIPS and LN

If you do not have the `ln` and `chips` running locally yet but would like to have that setup, see https://github.com/chips-blockchain/bet/blob/master/docs/HowToPangea.md.


## Run Docker container

    docker run --net=host --name bet -t -i -v /$HOME/.chips:/root/.chips:rw  -v /$HOME/.chipsln:/root/.chipsln:rw piggydoughnut/bet:v1.5

## Run bet
    
   Run the node that you want to run. Are you hosting a game? -> Run Dealer. Do you just want to play the game and someone is hosting it for you? -> Run Player.

   Check out [Tutorial](https://github.com/chips-blockchain/pangea-poker/blob/dev/tutorial/Tutorial.md)
   


<img src="https://media.giphy.com/media/jQWUkD7a4AWfkraBJa/giphy.gif" width="300" height="262" />
