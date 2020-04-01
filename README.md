# todo.txt-des

## Description

Add-on to add `description` into `todo.txt-cli` tasks.

**Disclaimer**: This tool is built to scratch my own itch: having details for a task. The tool was built in 2 hours in a hack-things-together manner so expect it to be (a little) buggy.

The `todo.txt` format does not specify anywhere to store **more** information related to a task.

This add-on introduces `description` concept in to the task by making use of [Additional File Format Definition](https://github.com/todotxt/todo.txt#additional-file-format-definitions).

## Concept

`todo.txt-des` add a `key:value` tag into your task, namely `des:xxxx` in which `xxxx` is a generated id to the file containing the whole description.

For example:

```bash
$ todo.sh ls
01 (A) Finish the whole @awesome-project on time. des:abcd938
```

> NOTE: as of 0.1.0, the `id` is the first 8 characters of SHA-256 of the original content and the ordinal of the task.

The description files are stored at `$TODO_DIR/descriptions/*.txt`

## Known Limitations

- When a task is marked as done by `todo.sh do $x`, the description file associating with `$x` is **NOT** deleted.

## Roadmap

> This TODO is using `todo.txt` format, of course.

```text
(B) Show the task's original content and id on the top when `show` +improvement.
(C) Allow to use an external editor when `add` the description as an +improvement.
```

## Installation

**You need `python3` in your `PATH`.**

Simply clone this repository into your `$TODO_ACTION_DIR` (defined in your `~/.todo.fg`)

```bash
$ mkdir -p ~/.todo.actions.d/des
$ git clone https://github.com/Genzer/todo-des.git ~/.todo.actions.d/des
$ chmod u+x ~/.todo.actions.d/des/des
```

## Usage

`todo.txt-des` supports 3 primary functions: `add`, `show` and `edit` (or use their aliases `a`, `s` and `e` respectively)

### Add a Description to a Task

> **IMPORTANT**: `add` a new description into a task which already has a description will effectively ovewrite the existing one. The content of the previous one, however, is kept intact.


```bash
# Simply a single text
$ todo.sh des add 1 "The project is defined to be TOP secret"

# Use heredoc
# Please note the second argument, the content of description, MUST be - so that the add-on knows to read from stdin
$ todo.sh des add 1 <<__DOC__
# Description

The project is defined to be TOP secret
__DOC__

# Or a file
$ cat some_prepared_text.md | todo.sh des add 1
```

## Show the Description

```bash
$ todo.sh des show 1
The project is defined to be TOP secret
```

## Edit the Description Using an External Editor

You can specify your preferred editor either through an environment variable `EDITOR` or `TODO_DESCRIPTION_EDITOR`.

> NOTE:
> The default editor is `vi`.
> The order of lookup is to search for `EDITOR` then `TODO_DESCRIPTION_EDITOR`.

For example:

```bash
# Your ~/.todo.cfg

# Use Visual Studio Code
export TODO_DESCRIPTION_EDITOR=/usr/local/bin/code
```

```bash
$ todo.sh des edit 1
# Use Code to open the file ~/.todo/descriptions/abcd012.txt
```


## Development

Please have a look at [Creating Add-ons: Examples](https://github.com/todotxt/todo.txt-cli/wiki/Creating-Add-ons%3A-Examples).

You need Python 3.6 or later.
There is no external dependencies.