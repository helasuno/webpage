# Welcome to Helasuno
Helasuno (hee-la-sue-no), coming from the (butchered) blending of the Esperanto words "bright" (hela) and "sun" (suno), is a simple non-structured and statement driven scripting language. Inspired loosely by BASIC and a desire to learn more about parsing text, Helasuno is a primitive scripting language by design. The project started being developed in 2024 but the first source release to the public was on 20/04/2025. The first packaged "stable" version is still a ways off yet.

!!! Note

    The language design is still under heavy revision. It is possible that convetions and design will change so don't be surprised if there is some sort of disconnect between what is here and what works (or doesn't). All efforts will be made to ensure alignment between the functionality of the interpreter and the documentation here.

### Quick Start
First and foremost, you need a working Python installation. The language is written in pure Python so a working version of Python installed that is >= version 3.10 will do.

If you just want to get going, you can run the following on a *nix (macOS, Linux, BSD) system:

    git clone https://github.com/helasuno/lang.git
    cd lang/
    python3 package.py

The above will create an executable Python zipapp in `dist/` that will work well across *nix platforms.

Using Helasuno is similar to any other interpreter:

    hs [name of script]

Downloads of the interpreter and Visual Studio Code will be made available once a release of each is finalised.


### Getting Started
The quick start above is really quick and you'll likely need some more detail. If that's you, head over to the packaging and setup guide.

If you're looking for details about the language itself, head over to the language definition page. A tutorial is also available via the tutorial page.

If you're interested in working on the language itself (ie. you don't want to write Helasuno scripts but want to help build the language itself), head over to the development page.

If you're looking for more details about the language, head over to the FAQ page.

### Repositories
The project is split across multiple git repositories.

| Repository | Description |
|----|----|
| [helasuno/lang](https://github.com/helasuno/lang) | This is the main repo with the interpreter itself. |
| [helasuno/media](https://github.com/helasuno/media) | This houses any media used in the project including icons and other art. |
| [helasuno/samples](https://github.com/helasuno/samples) | This houses sample scripts for testing and tutorials. |
| [helasuno/vscode_extension](https://github.com/helasuno/vscode_extension) | This is the home of the Visual Studio Code extension. |
| [helasuno/webpage](https://github.com/helasuno/webpage) | This is the mkdocs material for producing the project's website. |

## Licences
The code and materials are provided under either the MIT licence or are available as part of the public domain.

### MIT Licence
`Covers the source code for the interpreter (helasuno/lang), the VS Code extension (helasuno/vscode_extension), some (but not all) of helasuno/media, and the website (helasuno/webpage)`

Copyright 2024-2025 Bryan Smith.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

### Public Domain
`Covers some of the media (helasuno/media) and the sample code (helasuno/samples)`

Public Domain. The base icons come from the Tango Project and are available in the public domain. In the spirit of keeping things in the public domain as such, the combination of them into the project's logo/icon is also public domain.

The sample scripts are also public domain (do with them what you'd like).