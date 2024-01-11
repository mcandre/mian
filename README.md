# mian: a Scheme Lisp task runner

# ABOUT

mian enables Scheme developers to configure build tasks directly with Scheme code. This is a powerful advantage over the classic make utility. No longer do developers have to manage two completely different programming languages. With mian, Scheme devs have more expressiveness over their builds.

# EXAMPLE

```console
$ cd example

$ ./mian
Hello World!

$ ./mian test
Hello World!
```

# SETUP

mian demonstrates a basic task runner in Scheme. This example specifically targets Chicken Scheme, though you may write similar task runners for other Scheme implementations.

Inside the [example](example) directory, we have a simple `hello.scm` application representing a Scheme project. At the same level of the project is a `mian` task runner script.

Create a `mian` Scheme script file at the top level of your project. Omit any file extensions. Ensure the file receives executable chmod bits.

## Shebang

```scheme
 #!/bin/sh
 #|
 exec csi -s "$0" "$@"
 |#
```

The complex shebang above provides a POSIX compliant way to load the Scheme interpreter and query CLI arguments.

## Imports

```scheme
(import (chicken process-context))

; chicken-install shell
(require-extension shell)
```

This example uses the `process-context` Chicken standard library module, as well as a `shell` egg module. Chicken Scheme eggs must be installed separately via `chicken-install` commands.

## Create a task

```scheme
(define test
 (lambda () (run (csi -s hello.scm))))
```

mian declares tasks in the form of simple Scheme functions.

Note the shell module `run` function, which takes a list of command and argument literals, and executes them as a subprocess.

This example merely executes the `hello.scm` script. You will likely want to alter your project's `test` task to trigger a unit test suite.

A more typical Chicken Scheme project may feature `lint` tasks to detect coding quirks, `clean` tasks to remove build artifacts, and/or a `build` task to trigger compilation commands.

## Entrypoint

```scheme
(let ((default-task 'test)
      (args (map string->symbol (command-line-arguments))))
 (if (= (length args) 0)
  ((eval default-task))
  (map (lambda (arg)
        ((eval arg)))
        args)))
```

Above, we have a Scheme entrypoint. The entrypoint declares the symbol `'test` as the default task, simililar to the `all` default task convention in the make build system. You may choose another task symbol, whichever task is most relevant for your project.

When the user executes the shell command `./mian` with no arguments, then the default task processes.

When the user supplies some task arguments like `./mian test`, then the default task is ignored, and only the list of named tasks in the command line arguments will process.

## Further Research

Use a Chicken Scheme parser to extract the function names from its own script, then generate a usage message as a `help` task.

Validate command line arguments against the list of task names.

# SEE ALSO

* Inspiration from [nobuild](https://github.com/tsoding/nobuild), a convention for C/C++ build systems
* [bashate](https://github.com/openstack/bashate), a shell script style linter
* [beltaloada](https://github.com/mcandre/beltaloada), a shell build system
* [bb](https://github.com/mcandre/bb), a build system for (g)awk projects
* [Gradle](https://gradle.org/), a build system for JVM projects
* [jelly](https://github.com/mcandre/jelly), a JSON task runner
* [lake](https://luarocks.org/modules/steved/lake), a Lua task runner
* [Leiningen](https://leiningen.org/) + [lein-exec](https://github.com/kumarshantanu/lein-exec), a Clojure task runner
* [lichen](https://github.com/mcandre/lichen), a sed task runner
* [Mage](https://magefile.org/), a task runner for Go projects
* [npm](https://www.npmjs.com/), [Grunt](https://gruntjs.com/), Node.js task runners
* [POSIX make](https://pubs.opengroup.org/onlinepubs/009695299/utilities/make.html), a task runner standard for C/C++ and various other software projects
* [Rake](https://ruby.github.io/rake/), a task runner for Ruby projects
* [Rebar3](https://www.rebar3.org/), a build system for Erlang projects
* [rez](https://github.com/mcandre/rez) builds C/C++ projects
* [sbt](https://www.scala-sbt.org/index.html), a build system for Scala projects
* [Shake](https://shakebuild.com/), a task runner for Haskell projects
* [ShellCheck](https://www.shellcheck.net/), a shell script linter with a rich collection of rules for promoting safer scripting
* [slick](https://github.com/mcandre/slick), a linter to enforce stricter, unextended POSIX sh syntax compliance
* [stank](https://github.com/mcandre/stank), a collection of POSIX-y shell script linters
* [tinyrick](https://github.com/mcandre/tinyrick) for Rust projects
* [yao](https://github.com/mcandre/yao), a task runner for Common LISP projects

🍜
