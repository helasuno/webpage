# Packaging and Setup
This document walks you through getting started with setting up an environment for developing Helasuno scripts.

**Audience: script writers**

!!! Note

    The language and documentation are under heavy development. Don't be surprised if you come across errors in the documentation, code, or both.

## Getting the Code
As the languag continues to grow, the only way to get the code now is via the GitHub repository. The link to the repository is available in the top right corner of this page or via the following:

    git clone https://github.com/helasuno/lang.git

## Packaging The Interpreter
Before you get started writing a Helasuno script, you need to create a working version of the interpreter. While ideally a pre-packaged version would be provided with an elegant installer, the focus right now is on providing you with the minimum needed so that attention can be paid to working on the language itself. That said, you're not left on your own and two packaging methods are provided: a [packaging script](#package-script) and a [Makefile](#makefile).


### Package Script
This is the simplest solution in that it works across systems but it does lack some of the flexibility of the [Makefile](#makefile). If all you want to do is package the interpreter, this is a simple cross-platform solution.

To use this solution, run the `package.py` script:
    
    python3 package.py

This will create an executable zipapp in `dist/`. You can then just run that from the `dist/` directory or from wherever you happen to put it.

Once that is done, you will need to either:

- Put the interpreter somewhere on your PATH. On a *nix system (ie. non-Windows), you can find out what this is by executing the following: `echo $PATH`. Consult your operating system's conventions about placing files in the PATH (and when & where this is appropriate and safe).
- Put the interpreter somewhere safe like `~/.local/bin/` and add that to your PATH (see [here](https://phoenixnap.com/kb/linux-add-to-path) for some instructions).


### Makefile
On a more conventional *nix system (macOS/Linux/BSD), you can use the included Makefile.

    make

This will clean the `src/` directory, (re)compile the bytecode, and create the interpreter along with source distributions in `src/` directory. This is the same as:

    make clean bytecode package source

If you want to make the interpreter package and install it (in /usr/local/bin/), you can run the conventional pattern:

    make && sudo make install

You can pass parameters to the Makefile for more specific functions.

#### Makefile Parameters

| Parameter | Description |
|----|----|
| binary | If you have `nuitka` installed, you can create a binary of the interpreter. This works on macOS and FreeBSD. Not yet supported: NetBSD. |
| bytecode | Compile modules to Python bytecode in the `src/` directory. |
| clean | Clean up the source code directory of any lingering bytecode. |
| install | Copy the interpreter to `/usr/local/bin/`. This is *nix spcific and depends on the existence of the interpreter in `dist/` (run `make` first before running `make install`). |
| package | Package up the interpreter and place it in `dist/`. |
| source | Create zip file and bunzip'd tarball in `dist/`. |
