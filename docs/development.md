# Working on Helasuno
This document outlines how to work on the interpreter and thus, the language itself.

**Audience: language developers**

!!! Note

    The language and documentation are under heavy development. Don't be surprised if you come across errors in the documentation, code, or both.

## Conventions for the Code
* All language features follow spelling conventions from Australian English. Colour, for instance, is spelled with a *u*. American English spellings will be corrected if they do not map on to Australian English conventions.
* Commenting for functions follow Google's commenting style.
* Modularity is preferred; 10 scripts that are 100 lines long are eminently better than 1 script that is 1000 lines long.
* No third party modules. All code must be pure Python (ie. nothing outside of the standard library).
* Robust commenting is required which includes self-explanatory code. Given that the codebase is designed to help learners, there needs to be a clear articulation of what is happening with the logic.
* Tabs, not spaces. I know that PEP 8 notes that spaces are preferred but it also notes that tabs should be used when code already uses tabs. Here, tabs have been used so to remain consistent with PEP 8, use tabs.
    * PEP 8's recommendation for line length is adhered to however.



## src/ breakdown
The `src/` directory is broken down into a handful of directories that have specific purposes.

| Directory | Description |
|----|----|
| etc/ | This folder contains a variety of helper modules including the 'colouriser' (the module that handles colouring the output) and the global values module that holds globally available values. This directory also serves as "catch all" for modules that don't have a neat home elsewhere (ie. a module may be developed here and then moved if a design decision is made later that it fits better somewhere else). |
| maple/ | This is the parser that tokenises a script and creates the TOKEN_TREE, the object that is traversed and worked through in the execution of the script. No execution is done here but, rather, Maple serves to take a script and convert it into bits that are passed to statement modules that do the actual execution of functionality. More on [Maple](#maple-parser) below. |
| statements/ | There is one module for each statement in the language that is passed tokens from Maple that a statement module works on. For instance, the line `30 pause 3` is comprised of three tokens: the line number, the statement name (pause) and the value 3. The stmt_pause module would take these three tokens created by Maple and then pause the script for three seconds. |
| tests/ | This houses unittests. See [testing](#testing) for more. |
| tools/ | This is home to a variety of small companion tools. Some of these, such as the `reliner.py` script are also integrated into the interpreter itself. |


## Testing
Run `python3 -m unittest` in the `tests/` directory. All of them should pass.

## The Logic of Execution
This section details what happens once a user presses return to start execution. You can think of this as a roadmap for what happens to the script from start to finish (ie. the end of execution).

1. The `__main__.py` script handles the opening of the script and proceeds to call functions in a particular order to prepare the script for execution and then execute it.
2. The script is split into lines. This is a crucial first step as Maple often requires lines to be passed it as the basis for execution.
3. The first function called is the `arborist.run_arborist_checks()` function which runs through a series of checks to ensure that the script doesn't fall afoul of any language conventions or requirements. It checks the following in the following order:
    1. `arborist.check_unique_line_numbers()` - Check to see that there are unique line numbers for each line
    2. `arborist.check_sequential_line_numbers()` - Check that each line is sequential.
    3. `arborist.check_valid_line_numbers()` - Check that line numbers are valid line numbers.
    4. `arborist.check_for_end_statement()` - Check that the last line is an `end` statement.
    5. `arborist.check_for_multiples()` - Check that each line number is a multiple of the first line number.
    6. `arborist.check_for_nonzero_start()` - Check to ensure that the first line is not zero.
4. Next, the `arborist.prune_comments()` function removes any comments from the script.
5. Next, the tokens are built using the `planter.build_tokens()` function. This is nestled in an if statement block that checks to see if the user has requested that the performance of the token construction needs to be measured.
6. Next, the token tree is built via the `planter.build_tree()` function.
7. Once the tree is done, the interpreter has everything it needs to start executing. Internally, an elaborate Python dictionary is broken down into key-value pairs where the keys are the line numbers and the values are an array of tokens for that line. Given that we have what we need, we call `xylem.set_execution_location()` to tell the interpreter to start executing the script. The `xylem.set_execution_location()` function can take a parameter that is a line number (for something like the `jump` statement) but the default is to start at the beginning.
8. The `xylem.set_execution_location()` function loops over the token tree and makes calls to the `xylem.call_statements()` function for each line.
9. The `xylem.call_statements()` function does the work of calling modules from the `statements/` path. Here, if the second token on a line (remembering that the first would be the line number) was a `write` statement, the `stmt_write.stmt_write()` function would be called. Similar calls are made for each statement.

## Maple Parser
Maple is the language's parser. It takes in a list of lines of code and then works its magic by tokenising the script. Most of the interpreter's work is done with Maple aside from executing actual statement calls. Now, you may be asking yourself "if it doesn't do the actual execution of statements, what is it doing?" In short, Maple is the portion of the interpreter that does basic checks on the script and converts the script into something useful.

Let's look at an example. Let's say that we've got the following basic script:

    1 - This is a comment
    2 writeln "Hello World"
    3 end

The interpreter can't think like a human and quickly ignore the comment, see the "Hello World" as a thing to be printed to the screen, and move on. Rather, the script needs to be broken down into something that can be worked with. Maple does this work for us by converting this script into a set of tokens: an array of properties for significant parts of this script. Let's take a look at one token to see what is defined as significan

    {
        'full_line_of_code': '2 writeln "Hello World"\n',
        'script_line_number': '2',
        'token_end_location': 1,
        'token_number': 2,
        'token_start_location': 0,
        'token_type': (2, 'NUMBER'),
        'token_value': '2'
    }

Any given token, as you can see, is a dictionary containing information that is helpful for storing information about a portion of the script that is considered important. Let's break this down:

| Key | Value |
|----|----|
| full_line_of_code | This houses the full line of code that the token is on. This is useful in error reporting for instance. |
| script_line_number | This houses the line number we're on. As you can see, we are working with line number 2. |
| token_end_location | This signifies where the token that we are considered with ends. Since this happens to be the token for the line number, it ends as position 1. |
| token_number | Maple keeps track of how many tokens there are and this value tells us which token we're looking at. In this case, we are looking at the second token. |
| token_start_location | This tells us where the token starts. Since it is at the beginning of the line, we have a value of zero. |
| token_type | This tells us what type the token is. Here, we are working with a number. The first value in the tuple -- 2 -- is not the value of the token (this is a coincidence!) but a numeric representation of the token type. See [here](https://github.com/python/cpython/blob/3.13/Lib/token.py) for more. In our case, the number 2 just happens to be the numeric value for NUMBER tokens. |
| token_value | Finally, we have the value of the token itself. Here, since we are looking at the line number, we have the line number itself (ie. the value 2). |

### Maple Naming
While the keys in tokens above have relatively straightforward names, the modules in Maple use tree and plant related metaphors (since the parser is named after my favourite type of tree). 
* Each line of code is a *branch* (note: this is not a common language unlike those terms that follow that are extensively used).
* The *arborist* is the part of the parser that deals with checks and cleans things up much like an arborist would manage trees.
* The *planter* is the module that constructs tokens and then builds the token tree that is then used to execute commands much like someone would plant a tree.
* The *xylem* takes the lines of tokens (ie. a *branch*) and then starts calling statement module functions to execute the script much like the xylem in a tree transports nutrients to where they need to go.

## Statement Modules
The modules in `statements/` are individually responsible for handling statements.

Each module follows a particular naming convention:
* The file for any given statement is to be called `stmt_[statement name].py`
* Each module has a function called `stmt_[statement name]()` that takes one parameter: a list of tokens on that line.
    * Support functions can exist but `xylem.call_statements()` should call `stmt_[statement name].stmt_[statement name](tokens)`. For instance, the `end` statement is handled by `stmt_end.stmt_end(tokens)` in `xylem.call_statements()`.

Some statement modules only have their `stmt_[statement name]()` function. The `end` statement is a good example of this. Modules like the `stmt_set` one have support functions like `stmt_check()` work to ensure that an appropriate assignment operator is used and that the variable assignment doesn't try to create a variable with the same name as a statement or create one that starts with the prohibited prefix *hs_*.

## Tutorial: Writing a Statement
If you want to write a statement, this is the section for you.

In this section, we are going to implement a statement for Helasuno by adding a statement to Maple and writing a statement module. Specifically, we're going to write the `pause` statement as it is a rather simple statement but demonstrates the basics of working with Maple and writing a statement module.

### Part 0: A Simple Sample
For the purposes of this section, we are going to assume that we want to write *Hello World!* but do so with a pause after the *Hello*. We're also going to assume that the `pause` statement has yet to be implemented so we will need to write it.

For the sake of our statement implementation, we want the following script to work:

    10 write “Hello “
    20 pause 3
    30 write “World!”

We are also going to define a valid `pause` statement as having the form `pause [integer noting the number of seconds]`. So, `pause 3` is valid, `pause "for a short break"` is not.

Without writing the pause statement (assuming that it wasn't implemented), we'd encounter an error:

    Hello 
    [Error]
    [Line 20] The statement name "pause" is not a valid statement name.

So, let's fix that!

### Part 1: Registering the Statement
The first thing that we need to do is register the statement with Maple. In `maple/values.py`, find the **STATEMENT_NAMES** variable. Here, you will find a list that looks similar to the following:

    STATEMENT_NAMES = [
        'end',
        'get',
        'set',
        'write',
        'writeln'
    ]

This list houses the valid statement names for the language. Any statement used in a script that is not in this list will throw an error (`xylem.call_statements()` checks this). So, we first need to tell Maple that we want to register pause as a valid statement by adding it:

    STATEMENT_NAMES = [
        'end',
        'get',
        'pause’,
        'set',
        'write',
        'writeln'
    ]

If you run our sample script now, the script will run just fine but won't pause. Effectively, we've just told Maple not to throw an error when it encounters pause statements but that's it. In other words, we've told Maple that the `pause` statement is now valid but Maple doesn't do anything about any calls but just ignore it.

### Part 2: Write the pause Statement
Next up, we need to write the `pause` statement. Here, we need to create a module in the `statements/` folder called `stmt_pause.py`.

The first thing that we want to do is setup the boilerplate:

    #!/usr/bin/env python3

    # Standard library imports

    # Language imports
    from statements import stmt_set
    from tools import error

    '''Copyright 2024 Bryan Smith.

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the “Software”), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

    -- Description --
    This modules handles pausing the execution of the script.
    '''

Each module begins with the license and a short description. You’ll notice at the top that we import the `stmt_set` module from `statements/` and the error module from `tools/`. It will become clear shortly why that's the case.

Our first, and in this case only function, is going to follow a convention seen elsewhere: `def stmt_pause(tokens: list)`. The main function in a statement module is stmt_[statement name] and the first parameter is a type hinted list of tokens that will be passed by Maple. With that in mind, let’s start writing the function. First though, let's see what a set of tokens for a valid `pause` statement might look like:

    [
        {
            'full_line_of_code': '20 pause 3\n',
            'script_line_number': '20',
            'token_end_location': 2,
            'token_number': 6,
            'token_start_location': 0,
            'token_type': (2, 'NUMBER'),
            'token_value': '20'
        },
        {
            'full_line_of_code': '20 pause 3\n',
            'script_line_number': '20',
            'token_end_location': 8,
            'token_number': 7,
            'token_start_location': 3,
            'token_type': (1, 'NAME'),
            'token_value': 'pause'
        },
        {
            'full_line_of_code': '20 pause 3\n',
            'script_line_number': '20',
            'token_end_location': 10,
            'token_number': 8,
            'token_start_location': 9,
            'token_type': (2, 'NUMBER'),
            'token_value': '3'
        },
        {
            'full_line_of_code': '20 pause 3\n',
            'script_line_number': '20',
            'token_end_location': 11,
            'token_number': 9,
            'token_start_location': 10,
            'token_type': (4, 'NEWLINE'),
            'token_value': '\n'
        }
    ]

There are four tokens, only one of which is really necessary (the third one). The first token is the line number which is largely irrelevant for our work here (any errors with this will have been caught by the arborist). That said, it is helpful for reporting errors so we still want to account for it.

The second token is the `pause` statement call itself which is not necessary either (but is elsewhere which we will come back to later). The final token is a newline token; technically, each line ends with one but this is also inconsequential for us. That leaves us with the third token which is the length of the pause.

The first thing that we want to do is check that the value of the pause is a valid integer:

    def stmt_pause(tokens: list):
        # Get the requested pause length
        pause_length = tokens[2]['token_value']
        # Get the line number
        line_number = tokens[0]['script_line_number']
        # If the pause is not numeric...
        if not str(pause_length).isnumeric():
            # Throw an error
            error.simple_error(
                f'[Line {line_number}] The length of the pause requested ' +
                f’({pause_length}) is not a valid number.'
            )

The `pause_length` variable holds the value of the token from our third token and stores that. The `line_number` variable holds the line number that our `pause` statement is on which is helpful when we need to report back an error (say, for instance, that the value of the token is not valid). Finally, we've added a check to see if the `pause_length` is a valid number. If it isn't throw an error using the `simple_error()` function from the `tools/error` module.

Next, we need to implement the actual pausing of the script here. We’ll use Python's standard library `time` module here and the `sleep()` function. Let's implement that now:

    def stmt_pause(tokens: list):
        # Get the requested pause length
        pause_length = tokens[2]['token_value']
        # Get the line number
        line_number = tokens[0]['script_line_number']
        # If the pause is not numeric...
        if not str(pause_length).isnumeric():
            # Throw an error
            error.simple_error(
                f'[Line {line_number}] The length of the pause requested ' +
                f’({pause_length}) is not a valid number.'
            )
        # Pause the execution of the script
        time.sleep(int(pause_length))

Before continuing, make sure to import the `time` module at the top of the `stmt_pause` module.

At a very basic level, with some really simple error checking, we've implemented a `pause` statement that gives us enough to test with. Now we need to tell the `xylem.call_statements()` function to call the `stmt_pause()` function when it detects a `pause` statement. Let’s do that now. Open up `maple/xylem.py` and head over to the `call_statements()` function. Towards the bottom of that function, you will see a match-case that matches up statement calls with their corresponding statement module. Let's add a case for the `pause` statement:

    def call_statements(tokens: list):
        […]
            match statement_name:
                […]
                case 'pause':
                    stmt_pause.stmt_pause(tokens)
            […]

Before continuing, make sure to add `stmt_pause` to the list of imports in the `statements` import list at the top.

Here, all we are doing is adding a case for `pause` and calling `stmt_pause.stmt_pause()` and passing the tokens for that line. That’s it! Run the script and you will see that it works as intended. Now, this is not complete as we need to account for the possibility that someone will do some combination of the following:

* Using a mathematical expression in the pause statement that we will need to calculate
* Provide a variable that we need to parse

Let’s tackle those next.

### Part 3: Making the Pause Statement More Robust
Currently, the pause statement works but it's not particularly robust. For instance, if someone tries to pass a non-numeric string, it will error out but a user might rightly expect that they can pass a math expression to a pause statement and have it work. For instance, a user might reasonably assume that the following pause statement would work:

    10 write "Hello "
    20 pause "3+2"
    30 write "World!"

Here, we would expect the script to pause for 5 seconds between writing *Hello* and *World!*. However, if we do that, we will get the following output:

    Hello 
    [Error]
    [Line 20] The length of the pause requested ("3+2") is not a valid number.

This doesn’t work for two reasons:
1. We are passing a string here with a math expression in it. As a result, we need to change the function to read in strings as well and cast it to an integer.
2. We need to calculate the expression if it is one.

The is the first of our two problems (the other being variable substitution) but let's deal with this issue first before moving on. Helpfully, calculating an expression and doing variable substitution are easily done because Maple has a `helpers` module that provides two convenience functions: `substitute_values()` and `calculate_value()`. Note that this requires importing the `helpers` module from maple (ie. `from maple import helpers`).

Each of these functions are rather straightforward and do the heavy lifting for any conversions and substitutions needed. With this knowledge in hand, let’s see what our function now looks like (with some added function documentation) with our new parts:

    def stmt_pause(tokens: list):
        """Pause the execution of a script for a period of time

        Args:
            tokens [list]: the list of tokens on the line with the write statement
                call

        Returns:
            N/A

        Raises:
            None
        """

        # Get the requested pause length
        pause_length = tokens[2]['token_value']
        # Get the line number
        line_number = tokens[0]['script_line_number']

        # Substitute variables in the pause length
        pause_length = helpers.substitute_values(pause_length, line_number)
        # Calculate the expression if it needs to be calculated
        pause_length = helpers.calculate_value(pause_length)

        # If the pause is not numeric...
        if not str(pause_length).isnumeric():
            # Throw an error
            error.simple_error(
                f'[Line {line_number}] The length of the pause requested ' +
                f'({pause_length}) is not a valid number.'
            )

        # Pause the execution of the script
        time.sleep(int(pause_length))

### Part 4: Statement Modifiers
**TODO: This is next up.**