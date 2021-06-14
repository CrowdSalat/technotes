# screen

```bash
# create screen
screen -S bla

# leave/detach screen
# press: STRG+A D
# or when the screen is running elsewhere:
screen -d bla

# open screen
screen -x bla

# execute command in a new screen with the name blubb


#TODO execute command in a new screen, keep it open when the process is ended
screen -S blubb -dm echo "hello"; exec bash


# kill screen
screen -S blubb -X quit

# or connect to it and 
# press STRG+a  and type :quit

#TODO connect screen to running process

```