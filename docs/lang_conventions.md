# Language Definition
This document outlines:

- things to keep in mind while writing a script;
- information about variables including how to access them;
- information about statement modifiers;
- error code elaborations.


!!! Note

    The language and documentation are under heavy development. Don't be surprised if you come across errors in the documentation, code, or both.

## Conventions and Requirements
This section deals with parts of the language that are either worth considering or required to ensure that the script you write conforms with the syntax of the language.

### Comments
Comments in Helasuno are signified by a `-` that is sandwiched between a line number and the comment itself. [Noted below](#line-numbers), you will see that line numbers are mandatory so comments still need to be line numbered. With that in mind, a comment looks as simple as the following:

    10 - This is a comment

This line will be ignored by the interpreter (technically, the interpreter's parser, Maple, strips comments out of the script before it is parsed so they are less so ignored and more so removed before execution).

As of now, only single line comments exist.

### Ending a Script
Every script must include an `end` statement at the end. An error (error 5) will be thrown if one is not provided.

### Line Numbers
Each line of code *must* begin with a number of your choosing. There are a few features of these numbers that you must take into account:

| Property | Required | Comment |
|----|----|----|
| Integer | Yes | If you provide a float, the interpreter will yield an error. |
| Multiples | No | The interpreter will yield a warning if the line numbers are not multiples of the first.  It is best practice to run the interpreter with the `-r` flag as this will reline the script to follow convention (thus preventing this warning). This is a convention in the purest sense (that is, not mandatory). A warning (error code 6) will be thrown alerting you to this fact but execution will not be halted. You will notice, however, that the code still executes.  |
| Positive | Yes | Negative integers must not be used, otherwise the interpreter will yield an error. |
| Sequential | Yes | If the line number does not follow on from the previous number, the interpreter will yield an error. |
| Starts Greater than Zero | Yes | You need to start the script with a line number > 0. |
| Unique | Yes | Each line must be a unique number. It's helpful to think of this like an address in that each line needs a unique identifier so that it can be called. |
| Valid | Yes | Each line must have a valid line number to begin it. |


### Statement Density
Each statement must occupy it's own line. A line is read in its entirety and unlike other languages where you can put multiple lines on a single line (split by way of something like a semicolon), you must put each line of code on its own line with its own conformant line number.


## Variables
Variables are largely set via the `set` and `get` statements. They are not typed and a lot of faith is put into the user to produce valid values. Typing is on the radar as something that may come into play later down the line once the foundations of the language settle.

### Accessing Variables
To access a variable, you can use it in any string or value by prepending it with a *#* sign. For instance, if you want to get a variable called `name` and write it out, you could do the following:

    10 set name = "World"
    20 writeln "Hello #name"

On line 20, the variable `name` is included in the `writeln` statement because it is prepended by a *#* sign. This would output *Hello World* as a result.

### Reserved Variables: Prohibited Prefixes
Reserved variables are a set of variables that start with what is called the prohibited prefix *hs_*. You cannot name any variable in such a way that it starts with the prohibited prefix. An error will be thrown if you try (error 15). The prohibited prefix serves to guarantee that the language can grow with preset values that can be drawn on by blocking off a set of variable names for use internally in the language (and available to the script writer).

#### Available Reserved Variables
| Reserved Variable | Value | Version Availability |
|----|----|----|
| hs_current_date | The date in the format dd/mm/yyyy, for example, 29/03/2025. This is generated as of the invocation of the script, not when the variable is called. | 0.1- |
| hs_current_time | The time in the format hh:mm:ss, for example, 11:10:13. This is generated as of the invocation of the script, not when the variable is called. | 0.1- |
| hs_lang_name | The name of the language. | 0.1- |
| hs_lang_version | The version of the language being used. | 0.1- |


## Statement Modifiers
Some statements have things called *statement modifiers*, often called statmods. These are means to modify the functionality of a statement.

Statmods follow a statement after the statement modifier operator: `->`. They following the basic structure seen here:

    [line_number] [statement] [statement_value] -> [statmod]

An example for the `write` statement looks like the following:

    20 write "hello world" -> upper

The output of that will be HELLO WORLD as the *upper* statmod translates the output of a `write` statement to upper case.

Statmods can be "chained" if you pass them as a string and seperate them with a `|`. For example, the following will print "hello world" in upper case letters and green:

    20 write "hello world" -> "upper|green"

**NOTE: Some chaining requires particular statmods to be passed prior to others. For instance, the `write` and `writeln` statements require that statmods that change case come before those that change colours. You can know the appropriate order to place them in by following the ordering of them in statement definitions below.**


## Error Codes
If you run into an error or warning, you will notice that an error code is included. The table below elaborates on the error/warning message that you come across in case the error/warning message was not clear enough for you.

| Code | Type | Description |
|----|----|----|
| 1 | Error | This error is thrown when you try to interrupt something by pushing Ctrl-C in your terminal. It's likely the case that the user knew this error was coming. |
| 2 | Error | This error is thrown when you don't have unique line numbers. A key requirement of a script is that each line has a unique line number. Look for the multiple line numbers that are the same as reported by the error. |
| 3 | Error | This error is thrown when the lines of code are not sequential. Each line needs to have a line number greater than the past |
| 4 | Error | This error notes that a line does not start with a valid line numbers, that is, an integer greater than zero. |
| 5 | Error | This error is thrown when there is not a valid `end` statement as the last statement in the script. |
| 6 | Warning | This warning is thrown when there are line numbers that are not multiples of the first line. |
| 7 | Error | This error is thrown when there is a zero as a first line number. You need to use a non-zero positive integer as the first line.  |
| 8 | Error | This error occurs when Maple, the language parser, errors out because there is a part of the script that can't be accounted for. An error message will be provided to guide this rather general error which is commonly caused by a string that is missing a quotation mark. |
| 9 | Error | This error is thrown if the script passed to the interpreter can't be found. It's possible that either a typo was made or the file isn't accessible (in spite of it existing). |
| 10 | Error | Thrown when the interpreter is called without a script. |
| 11 | Error | This error happens when a variable value is requested but it isn't set. Make sure that the variable is set before continuing along. |
| 12 | Error | The statement you've provided isn't valid, perhaps a valid one spelled wrong or just a mistake. |
| 13 | Error | The `end` statement that you've provided involves too many options and values. The only thing that should be on that line is a line number and the `end` statement itself. |
| 14 | Error | The `pause` statement includes a parameter that isn't a valid number, therefore the statement is trying to pause on the basis of an invalid timeframe. |
| 15 | Error | The `set` statement tries to create a variable using the prohibited prefix (see `set` statement). You need to start your variable with a different prefix. |
| 16 | Error | The error here is calling attention to the lack of an appropriate operator used to assign a variable. A `set` statement should be followed by the variable name, an equal sign, and the value of the variable. |
| 17 | Error | This error is thrown when the user tries to name a variable after a statement (eg. naming the variable write). |
| 18 | Error | The wrong operator is used to modify a statement. You need to use `->`. |
| 19 | Error | An invalid statement modifier was provided to a `write` or `writeln` statement. A list of valid statement modifiers will be provided in the error message. Perhaps this was just a typo? |
| 20 | Warning | A statement modifier (statmod) is placed in the wrong order. While this may seem irrelevant, some statmods need to be ordered in a particular way. See the documentation for the statement being modified for more details. |
| 21 | Error | You passed something to a `jump` statement that isn't a valid number. |
| 22 | Error | This error is thrown if the script is stuck in a loop and if there's no certain break from the loop. Check for any `jump` statements that might create an unending loop. |
| 23 | Error | An invalid line number is passed to a `jump` statement. Unlike error 21, where the error is thrown if something like "ab" is passed (ie. not a number), this error is thrown if a valid line number is provided but if the number doesn't exist. |
| 24 | Error | This error occurs when you try to substitute a variable but forget the name, that is, you just provide the symbol (eg. `30 writeln "Hello #"`). Fixing this is as simple as making sure all variable names are accounted for. If you meant to include just the `#` symbol, you need to escape it by providing it twice: `##`. |