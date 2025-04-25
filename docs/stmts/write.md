The `write` and `writeln` statements output text to the screen, similar to the *print()* function in Python.

The difference between the `write` and `writeln` statements is simple:

- `write`: write text to the screen but do not add a new line at the end, allowing any following output to appear on the same line
- `writeln`: write text to the screen and add a new line at the end, forcing any following output to appear on the next line


### Structure

#### write
> [line number] write "[output]"

#### writeln
> [line number] writeln "[output]"


### Statement Modifiers
The statmods below need to be accessed in the order that they are provided. For instance, if you want a lowercase and green string to be written to the screen, you will need to chain them in the following order: *"lower|green"*

| Statmod | Description |
|-----|-----|
| lower | Convert the string to be printed to lower case |
| upper | Convert the string to be printed to upper case |
| blue | Write the string to the screen in blue |
| green | Write the string to the screen in green |
| magenta | Write the string to the screen in magenta |
| red | Write the string to the screen in red |
| yellow | Write the string to the screen in yellow |


### Example
The script below will output "HelloWorld" on one line as the `write` statement on line 10 won't push the next output to a new line. The `writeln` call on line 20 will push the next output to a new line so line 30 - *Hi there!* - will appear on its own line.

    10 write "Hello"
    20 writeln "World"
    30 writeln "Hi there!"
    40 end