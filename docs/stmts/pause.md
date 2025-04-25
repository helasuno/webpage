The `pause` statement pauses the script for a specified number of seconds. 


### Structure
> [line number] pause [pause_length]

The `pause_length` can be an integer or a string (to pull in variables or calculate expressions for instance).


### Statement Modifiers

None.


### Example
The following script will pause for three seconds between writing *Hello* and *World* to the screen.

    10 write "Hello "
    20 pause 3
    30 write "World"
    40 end