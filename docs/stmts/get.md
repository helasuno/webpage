The `get` statement facilitates getting user input and assigning it to a variable. It has a structure similar to the `set` statement but instead of assigning a variable in code, the value of the variable is set based on a prompt put to the user.

You can name your variable with any name other than the following exceptions:
- A variable cannot be named after a builtin statement.
- A variable cannot start with what is called the prohibited prefix (`hs_`). These are for reserved variables. See the language conventions for more on reserved variables.

### Structure

> [line number] get [variable_name] = "[prompt_to_get_input]"


### Statement Modifiers

None.


### Example
The following script will prompt the user for their name (via the `get` statement) and then write a greeting to the screen.

    1 get name = "What is your name?"
    2 write "Hello #name"
    3 end