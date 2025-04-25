The `set` statement assigns a value to a variable. There is no type checking here and any value can be assigned to a variable.

You can name your variable with any name other than the following exceptions:
- A variable cannot be named after a builtin statement.
- A variable cannot start with what is called the prohibited prefix (`hs_`). These are for reserved variables. See the language conventions for more on reserved variables.


### Structure
> [line number] set [variable_name] = "[variable_value]"


### Statement Modifiers
None.


### Example
The following script will set a variable called `name` which is accessed in the `write` statement which outputs *Hello World* to the screen.

    10 set name = "World"
    20 write "Hello #name"
    30 end