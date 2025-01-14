# Note From Clarence-pan
I was very happy using `parallelshell` to run scripts in parallel, until today I upgraded node.js to v10. Unfortantely [darkguy2008/parallelshell](https://github.com/darkguy2008/parallelshell) seems stopped maintenance.

So I forked it, and published a scoped package: `@clarencep/parallelshell`.

Install it: `npm install -g @clarencep/parallelshell`.

Now you can use it as the original parallelshell.

-----

## Parallel Shell

This is a super simple npm module to run shell commands in parallel. All
processes will share the same stdout/stderr, and if any command exits with a
non-zero exit status, the rest are stopped and the exit code carries through.

### Version compatibility notes

* Fully compatible with Node up to v8 and later!

### Maintenance has been resumed by [@darkguy2008](https://github.com/darkguy2008). However, there are also better options, see [Consolidation of multiple similar libraries](https://github.com/mysticatea/npm-run-all/issues/10).

### Motivation

**How is this different than:**

    $ cmd1 & cmd2 & cmd3

* Cross platform -- works on Unix or Windows.

* `&` creates a background process, which only exits if you kill it or it ends. `parallelshell` will autokill processes if one of the others dies.

* `command1 & command2 & command3` will wait in the terminal until command3 ends only. parallelshell will wait until all 3 end.

* If command1 or command2 exit with non-zero exit code, then this will not effect the outcome of your shell (i.e. they can fail and npm/bash/whatever will ignore it). `parallelshell` will not ignore it, and will exit with the first non-zero exit code.

* Pressing Ctrl+C will exit command3 but not 1 or 2. `parallelshell` will exit all 3

* `parallelshell` outputs all jobs stdout/err to its stdout/err. background jobs do that... kind of coincidentally (read: unreliably)

**So what's the difference between GNU parallel and this?**

The biggest difference is that parallelshell is an npm module and GNU parallel isn't. While they probably do similar things, albeit (GNU) parallel being more advanced, parallelshell is an easier option to work with when using npm (because it's an npm module).

If you have GNU parallel installed on all the machines you project will be on, then by all means use it! :)

### Install

Simply run the following to install this to your project:

```bash
npm i --save-dev parallelshell
```

Or, to install it globally, run:

```bash
npm i -g parallelshell
```

### Usage

To use the command, simply call it with a set of strings - which correspond to
shell arguments, for example:

```bash
parallelshell "echo 1" "echo 2" "echo 3"
```

This will execute the commands `echo 1` `echo 2` and `echo 3` simultaneously.

Note that on Windows, you need to use double-quotes to avoid confusing the
argument parser.

Available options:
```
-h, --help         output usage information
-v, --verbose      verbose logging
-w, --wait         will not close sibling processes on error

```
