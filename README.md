`bpm` - Bash Package Manager
============================

Welcome to the simplistic Bash package manager, allowing you to install libraries and programs easily.

Programmers are hired because of their experience and they also bring with themselves various collections of functions and libraries. When writing a lot of code, there are certain patterns that you fall into and repetitious tasks where you do not want to repeat yourself, so you copy and paste into your new program. That quickly becomes difficult to maintain. This duplicated code is eventually broken out into a library and programs include the functionality instead of duplicating it, easing the burden of maintenance.

A problem with Bash is that it doesn't have a package manager, unlike other languages like Go and Node.js. Also, there's no convenient way to scour folders and find libraries that may be installed. `bpm` is a package manager to help download and include libraries for Bash, attempting to mitigate both of these problems.


Installation and Upgrade Instructions
-------------------------------------

To install for everyone on the system, run these commands. This folder should already be in your `$PATH`, so it should be searched automatically when running commands. Other users on the system won't need to do anything to run this program.

    curl -sSO https://git.io/bpm
    chmod 755 bpm
    sudo mv bpm /usr/bin/

The other option is to install it just for you into your own `bin/` folder. This assumes you've already created a `bin/` folder. You almost certainly need to add your `bin/` folder to `$PATH` so it is searched for commands. Other users on the system will also need to do the same if they want to run any commands that use `bpm` to find libraries.

    curl -sSO https://git.io/bpm
    chmod 755 bpm
    mv bpm ~/bin/


### Adding to `$PATH`

`bpm` relies on being in the path in order to load libraries easily.

If you chose to install it globally, then `/usr/bin/` may already be in your `$PATH`. When installing it locally, you often need to add your `bin/` folder to the `$PATH`. You can verify it by using this command:

    echo "$PATH"

If the folder is there, you're done. Otherwise, you need to add a line to either your `.bashrc` or `.profile` (it depends on the system) that looks like one of the following.

    # If a global install, add it last.
    PATH="$PATH:/usr/bin"

    # If a user install, add it first. You also need to add the location
    # where commands get installed.
    PATH="$HOME/bin:$HOME/.bpm/.bin:$PATH"


### Upgrading

Upgrading `bpm` is the same process as installation.

To determine which installation method was followed, use `which bpm` and look at the path where `bpm` was installed.


### Uninstalling

The instructions are different for user and global installs.  First, let's deal with global installs.

    # Delete the program.
    sudo rm /usr/bin/bpm

    # Delete libraries.
    sudo rm /usr/lib/bpm/

    # Delete programs that were installed. These are wrapper scripts and they
    # all will contain "command file for bpm".
    grep "command file for bpm" -rZ --binary-files=without-match /usr/bin | xargs -0 sudo rm

For user installs, the commands are slightly different.

    # Delete the program
    rm ~/bin/bpm

    # Delete libraries and installed commands.
    rm -rf ~/.bpm/

    # Edit your .bashrc or .profile and remove the $PATH entry

That last step is a manual one to remove the lines that were added to update your `$PATH` environment variable.


How Does It Work?
-----------------

Well, like many other programs out there, you can get a help message by running `bpm help`. It will tell you the limited amount of commands it supports.


### `bpm help`

This displays a simple help message.


### `bpm install [OPTIONS] [PACKAGE [...]]

Installs packages. When there are no packages nor options on the command-line, this searches a `bpm.ini` file and installs all of the `[dependencies]` and `[devDependencies]` that are listed.

With any arguments, the listed packages will be installed. They are installed into a parent folder that contains a [`bpm.ini` file][INI file]. If no parent is found, it will install into your local package folder, `~/.bpm/`.

Using the `--global` or `-g` flags will install the listed packages into the system's package folder. You'll probably need to also use `sudo` when you use `--global`.


### `bpm run <SCRIPT> [ARGUMENT [...]]`

Run a script from a `bpm.ini` that is in the current folder or any parent folder. Script names are case-insensitive, and are in the `[scripts]` section of the [INI file]. Arguments are all passed to the script.


How to Leverage `bpm` Modules
-----------------------------

This was made to be super simple for shell scripts.  First, you need to source `bpm` in order to get the loader functions, then start loading libraries.

    #!/usr/bin/env bash
    # Load bpm include function. This works because bpm is in $PATH.
    . bpm

    # Now load necessary libraries. This loads the string library.
    bpm::include string

Pretty easy, but this program doesn't have the string library available. Let's install it.

    # Use this when bpm is installed globally.
    sudo bpm install -g string

    # Use this when you are using a user install.
    bpm install string

    # Use this if you want to make an INI file and list dependencies with the program.
    bpm install

That last command requires the use of an [INI file]. Here is the absolutely smallest one to install the `string` library.

    [dependencies]
    0=string

There's a lot more that can go in this file, and it's all [documented][INI file].


Limitations
-----------

This is a very simplistic package manager. If the project gains some steam, it is entirely possible that these limitations would be overcome by later revisions and contributions.


### No Versioning

Because Bash does not have any sense of namespacing, libraries must be always backward-compatible or offer a significant period of time where the API can change. This is a huge negative, but does make the package dependency resolution significantly easier.


Other Alternatives
------------------

*Deps* is if the tool supports dependencies. **Locations** is where libraries can be installed (globally, in a user's home folder, or in a specific repository).

| Software | Deps | Versions | Locations |
|---|---|---|
| basher | Yes | No | Global, User |
| [bpkg] | Yes | Yes | Global, Repo |
| `bpm` | Yes | No | Global, User, Repo |
| [Jean] | No | No | Global |
| [pkgtool] | Yes | Yes | Global |
| [Tarp] | Yes | No | Global |

* [basher] - No central repository required; uses GitHub repositories. Thoroughly tested code. Includes completions. Geared much more for binary programs instead of Bash libraries. Keeps the package repositories.
* [bpkg] - Lightweight Bash package manager. Centralized repository for searching and installs inside of a project's "deps" folder. Confusingly, it uses `package.json` like Node.js projects. Meant more for executables.
* [Jean] - The missing shell package manager for Linux. List of packages is stored locally (`packages.txt`) and currently lists 4 packages. Looks like it is missing anything to tie the scripts together, like what the "main" property does for an npm-style `package.json`. Really dislikes spaces in folder names.
* [pkgtool] - Developed by Slackware. Installs tarballs to the system.
* [Tarp Package Manager][Tarp] - Installs tarballs to the system. Deprecated code and developer is off the net.


[basher]: https://github.com/basherpm/basher
[bpkg]: http://www.bpkg.sh/
[INI File]: doc/bpm-ini.md
[Jean]: https://github.com/ziyaddin/jean
[pkgtool]: http://www.slackbook.org/html/package-management.html
[Tarp]: https://code.google.com/archive/p/tarp-package-manager/
