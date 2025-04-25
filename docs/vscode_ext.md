## Visual Studio Code Extension
Available in the [vscode_extension](https://github.com/helasuno/vscode_extension) repo is the source for a Visual Studio Code extension which adds some basic language support. It's nothing fancy but it does provide syntax highlighting and snippet support to ease the creation of scripts.

### Packaging and Installing
First, get the code:

    git clone https://github.com/helasuno/vscode_extension.git

Packaging the extension requires having vsce installed. If you don't have that installed, you will need to do so:

    npm install -g @vscode/vsce

If it's the case that you don't have `npm` installed, you will need to do that as well.

* On macOS with Homebrew, a simple `brew install npm` will do the job.
* On Linux, something like `dnf install nodejs-npm` will work on Fedora and `apt install npm` will work on Debian and Ubuntu.

Once that is done, you can run the following:

    mkdir -p dist/
	cd src/
    vsce package --allow-missing-repository
	mv helasuno*.vsix ../dist/

In the `dist/` directory that now exists, you should see a `.vsix` file. In VS Code, open up the Command Palette and select the `Extensions: Install from VSIX...` option, select the VSIX file and enjoy.

### Using the Extension
The extension offers two primry features: syntax highlighting and snippets.

The syntax highlighting works automatically on any file with a `.hls` extension.

Code snippets help to facilitate not just statement writing but line numbering as well. Typing the following will allow you to trigger snippets:

| Snippet Command | Output |
|----|----|
| co | [auto line number] - [empty space for comment] |
| end | [auto line number] end |
| ge | [auto line number] get [empty space for variable name] = "[empty space for prompt]" |
| ju | [auto line number] jump [empty space for line number] |
| pa | [auto line number] pause [empty space for pause length] |
| se | [auto line number] set [empty space for variable name] = "[empty space for prompt]" |
| wr | [auto line number] write "[content to write]" |
| wl | [auto line number] writeln "[content to write to a line]" |