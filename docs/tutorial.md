# Tutorial
This document will walk you through writing an increasingly complex Helasuno script, learning the conventions of the language as you go.

**Audience: script writers**

!!! Note

    The language and documentation are under heavy development. Don't be surprised if you come across errors in the documentation, code, or both.

## Checking the Interpreter
To get started, we need to check that we have access to the interpreter. Let's check by executing the following:

    hs -v

If you installed the interpreter with the Makefile (`make && make install`) on a *nix system, you should be good to go and see something like the following:

**macOS**

    Helasuno 0.1 (Darwin, arm64)
    Python 3.13.2

**Linux**

    Helasuno 0.1 (Linux, aarch64)
    Python 3.13.2


**FreeBSD**

    Helasuno 0.1 (FreeBSD, arm64)
    Python 3.11.11

**NetBSD**

    Helasuno 0.1 (NetBSD, evbarm)
    Python 3.13.2



On Windows, you will need to call python to run the package:

    python.exe .\hs -v

That should yield something similar to the following:

    Helasuno 0.1 (Windows, ARM64)
    Python 3.13.2


### Using the Interpreter

    hs [script_name]

You may also see the following suggested as a common pattern for running scripts:

    hs -r [script_name]

The `-r` flag will reline the script before executing it, helping to prevent warnings from appearing that the script doesn't conform to line numbering conventions. You may see this commonly used as this will help to keep the numbering conformant with only a minimal hit to performance. In light of that, we will be running any scripts below with the `-r` flag from here out.


## Tutorial 1: Hello World
It wouldn't be appropriate to start with anything but a "Hello World" script. Let's start by looking at a simple "Hello World" script:

    10 - The script is public domain. This is a test.
    20 writeln "Hello World"
    30 end

Let's break this script down:
1. The lines are numbered much like they are in BASIC. These are mandatory and you will encounter an error if you don't use these. See the Language Definition > Conventions > Line Numbers section for more.
2. [Line 10] The first line is a comment, signified by the dash after the line number. You can think of the dash as telling the interpreter to "subtract" that line from consideration.
3. [Line 20] The `writeln` statement does the "Hello'ing" so to speak in printing out the string *Hello World* to the screen.
4. [Line 30] The script concludes with an `end` statement call. These are mandatory.

### Statement Modifiers (Statmods)
Some statements can be modified to change their behaviour. Above, we've got ourselves a rather plain greeting for our user. Let's try to add some colour. To do this, we can use a statement modifier, often called a statmod, to modify the `writeln` statement. Let's tweak the above:

    10 - The script is public domain. This is a test.
    20 writeln "Hello World" -> green
    30 end

Let's focus on line 20. Here, you'll see that we added `-> green` to the end to modify the `writeln` statement. If you were to run this, you'd see *Hello World* printed in green in your terminal.

Broken down, we have two parts:
* `->`: the statement modifier operator. This tells the interpreter that what is coming will modify the statement beforehand. Imagine as though this is you saying "do this to the statement call."
* green: This is the statement modifier itself. In this case, we chose something simple in just changing the colour of the output.

#### Chaining Together Statmods
In some select circumstances, you can chain statmods to perform multiple modifications. Let's say that we want to be really enthuasiastic in our greeting for the user. We might signal that by writing in all caps. We can use the `upper` statmod for the `writeln` statement here. Rather than just replacing the `green` statmod, we can chain them by formatting our statmod specifically. Let's take a look at our revised `writeln` statement:

    writeln "Hello World" -> "upper|green"

Here, we've chained `upper` and `green` together. When we run this now, we'll see HELLO WORLD printed in upper case letters in green in the terminal window. Neat!


## Tutorial 2: Hello [Insert Name]
Let's add a twist to our example above by personalising the output to the user. Here, we're going to introduce the `get` statement, a simple statement that collects user input and stores the value in a variable. We can use that value and write back a personalised greeting.

Let's take a look at the `get` statement first. The `get` statement takes the following form:

    get [variable name to hold the input] = [prompt used to solicit user input]

An example of the above might look like the following:

    get name = "What is your name? "

The `get` statement above will ask the user for their name and store the value in a variable called *name*. Let's revise our script above to include the `get` statement:

    10 - The script is public domain. This is a test.
    20 get name = "What is your name? "
    30 writeln "Hello World" -> green
    40 end

The script above gets us on the right path and if you run it, you'll be prompted for your name but the green coloured "Hello World" will be printed. You will notice above that the reason for this is quite simple: we ask for the user's name but then do nothing with it. Here, we need to use variable substitution in our `writeln` statement, something done by using the variable name in the string printed prepended by a *#*:

    10 - The script is public domain. This is a test.
    20 get name = "What is your name? "
    30 writeln "Hello #name" -> green
    40 end

With this change made, your script will output a greeting that is tailored to the user.


## Tutorial 3: Addition Calculator
Our next example is going to introduce you to the `set` statement. We're going to combine this new knowledge with previously learned materials. Here, we are going to get two numbers, add them, store the result in a variable, and then write out the answer. Let's start with our script:

    10 - The script is public domain
    20 get first = "First Number: "
    30 get second = "Second Number: "
    40 set answer = "#first + #second"
    50 writeln "#first added to #second is #answer."
    60 end

Lines 10-30 and 50-60 should be familiar. In any case, let's break the script down line-by-line:

- Line 10: the first line is a comment and this line is discarded
- Line 20 and 30: the two `get` statements get a number each
- Line 40: this is our new addition - the `set` statement - which is a simple variable assignment. The statement here takes on a similar form to the `get` statement but instead of the assignment creating a prompt, the assignment assigns a value to a variable. You'll also notice here that the *answer* variable is a math expression; if something can be calculated in Helasuno, it is.
- Line 50: write out our answer
- Line 60: end the script

### Tweaks
Try a few different tweaks to this script:

1. Try letting the user input the operator. Here, we've hardcoded the `+` operator into the *answer* variable assignment.
2. Add some colour to the output.