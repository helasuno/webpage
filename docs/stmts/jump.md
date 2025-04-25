The `jump` statement facilitates jumping around the script by line number. This is similar to the `goto` statement that you might see in other languages.

A note on jump statements needs to be made. Jumping in code via statements are controversial. As [Sanfoundry](https://www.sanfoundry.com/c-question-goto-statement-pros-cons/) puts it: "By using gotos excessively, you create a labyrinth of program flow. If you aren’t familiar with gotos, keep it that way. If you are used to using them, try to train yourself not to." Or, put more humourously by XKCD:

![go to dinosaur comic from XKCD](https://imgs.xkcd.com/comics/goto.png)
Comic courtesy of xkcd. Licensed under the Creative Commons Attribution-NonCommercial 2.5 License: [https://creativecommons.org/licenses/by-nc/2.5/](https://creativecommons.org/licenses/by-nc/2.5/).

I fully acknowledge that the inclusion of such a statement is not a good idea for moving around a script. It is hoped that this will be removed in the future with something better but for now, this is how you can move across a script.


### Structure

> [line number] jump [line_number]


### Statement Modifiers

None.


### Example
The following script will output *World* to the screen as line 30 would be jumped over.

    10 - The script is public domain
    20 jump 40
    30 writeln "Hello"
    40 writeln "World"
    50 end