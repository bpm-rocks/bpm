`bpm.ini` File
==============

BPM can base all of its operations off of a `bpm.ini` file, which lists information about a particular software package. The INI file format was chosen because of the easy format for editing, the ability to store comments, the clean separation between data and code, and because it can be parsed with pure Bash.

Here's a short example that shows several features. Each INI file section is explained more thoroughly below.

    [package]
    name=sample
    version=1.0.0
    include=libsample
    description=A sample package to show how packages work.
    license=MIT

    [dependencies]
    assign=*

    [devDependencies]
    embed-docs=*
    unittest=*

    [scripts]
    readme=embed-docs README.md libsample
    test=unittest test/*.test

    [install]
    sample=sample

Section names and keys are not case sensitive, thus `[devDependencies]` is the same as `[DEVDEPENDENCIES]` and `name=sample-package` is the same as `NAME=sample-package`. The values are case sensitive.


`[package]`
-----------


`[dependencies]`
----------------


`[devDependencies]`
-------------------


`[scripts]`
-----------


`[bin]`
-------

