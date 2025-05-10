The `pause` statement pauses the script for a specified number of seconds. 


### Structure
> [line number] pause [pause_length]

The `pause_length` can be an integer or a string (to pull in variables or calculate expressions for instance).


### Statement Modifiers
The statmods below need to be accessed in the order that they are provided.

**Max Statmods: 1**

#### Counting
Only one of these are allowed
| Statmod | Description |
|-----|-----|
| countdown | Countdown in the form of a simple message with the format `[seconds]...` |



### Example
The following script will pause for three seconds between writing *Hello* and *World* to the screen.

    10 write "Hello "
    20 pause 3
    30 write "World"
    40 end