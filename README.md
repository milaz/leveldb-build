# leveldb-build

Collection of scripts to build snappy, leveldb and levigo with GNU toolchain

## What is it

At the time I decided to set this project up, it is not a breeze to build
Google's LevelDB itself, and when it comes to build it with Snappy, things
become more complicated. More of it when you have to use LevelDB in your
project with language bindings.

This is a collection of scripts to facilitate and automate the process of
building snappy-enabled LevelDB and various language bindings.

The libraries that are built with these scripts are not installed system-wide.
Instead, an isolated environment is set up where you can experiment with
certain versions of these libraries and their bindings. 

## How to use

### In short

```
./ctl/setup
source ./ctl/go-env

go get github.com/jmhodges/levigo
go test github.com/jmhodges/levigo

mkdir -p ./src/project
cd !$
```

And start hacking ;)

### Detailed description

This collection of scripts is intended to be used with GNU toolchain. For
specific commands that must be installed on your system, see "Prerequisites"
section below.

Obtain the collection of scripts in a usual way: 
`git clone https://github.com/milaz/leveldb-build.git`

Enter the directory: `cd leveldb-build`

Issue the command that fetches and builds the latest LevelDB with Snappy
support:

`./ctl/setup`

Now, after the libraries are built, you can set up environment for Go
development. Issue a `source ./ctl/go-env` in your shell, and all subsequent
commands in that shell will use this directory as a Go Workspace, and also
will use the correct locations of LevelDB shared library and header files.

After that, getting and testing Levigo is done in a usual way:

```
go get github.com/jmhodges/levigo
go test github.com/jmhodges/levigo
```

Next time you open a shell and want to continue the development, change
directory to `leveldb-build` and issue a `source ./ctl/go-env` command.

Also, you can set up as many copies of `leveldb-build` as you wish. Just issue
`./ctl/setup` in the copy, and you are ready to go.


## Prerequisites

You must have SVN and Git so that setup script can fetch Snappy and LevelDB.
Other than that, the usual GNU toolchain is required: GNU Compiler Collection,
libtool, automake, autoconf, make.

## To do

Presently, only Levigo is supported. More language bindings to be expected in
the future.

## Credits

In discussion of 
[LevelDB issue #27](https://code.google.com/p/leveldb/issues/detail?id=27), 
Alessio Treglia, Kim Altintop and Jeff Hodges proposed patches that allow
building LevelDB as a shared library.

I found 
[instructions by Hiram Chirino](https://github.com/fusesource/leveldbjni/blob/master/readme.md) 
very helpful to get going on building LevelDB with Snappy support.

Finally, at 
[LevelDB issue #94](https://code.google.com/p/leveldb/issues/detail?id=94), 
JT Olds pointed out that Snappy is not linked to the resulting
`libleveldb.so`. Sadly, while he proposed the patch, it is left without due
attention. That leads to a lot of strange situations, e.g. inability to use
leveldb-haskell in ghci, or having to haul Snappy sources everywhere along
with LevelDB, while the latter does not export anything from the former.

The aforementioned efforts made it possible to make this small script
collection. The author hopes that it will help those who want to concentrate
on developing LevelDB-based programs, but are not willing to spend
considerable amount of time learning the kinds of libraries and ways of
linking them on *nix systems.

