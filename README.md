# todo.txt-note

## Description

Add-on to add `note` into `todo.txt-cli` tasks.

NOTE: Disclaimer: This tool is built to scratch my own itch: having details for a task. The tool was built in 2 hours in a hack-things-together manner so expect it to be (a little) buggy.

The `todo.txt` format does not specify anywhere to store **more** information related to a task.

This add-on introduces `note` concept in to the task by making use of [Additional File Format Definition](https://github.com/todotxt/todo.txt#additional-file-format-definitions).

## Concept

`todo.txt-note` add a `key:value` tag into your task, namely `note:xxxx` in which `xxxx` is a generated id to the file containing the whole description.

For example:

```bash
$ todo.sh ls
01 (A) Finish the whole @awesome-project on time. note:abcd938
```

> NOTE: as of 0.1.0, the `id` is the first 8 characters of SHA-256 of random sequence of bits.

The description files are stored at `$TODO_DIR/notes/*.txt`

## Known Limitations

- When a task is marked as done by `todo.sh do $x`, the note file associating with `$x` is **NOT** deleted.

## Roadmap

> This TODO is using `todo.txt` format, of course.

```text
(A) Delete orphan note files.
(B) Show the task's original content and id on the top when `show` +improvement.
(C) Allow to use an external editor when `add` the description as an +improvement.
```

## Installation

**You need `python3` in your `PATH`.**

Simply clone this repository into your `$TODO_ACTION_DIR` (defined in your `~/.todo.fg`)

```bash
$ mkdir -p ~/.todo.actions.d/note
$ git clone https://github.com/Genzer/todo-note.git ~/.todo.actions.d/note
$ chmod u+x ~/.todo.actions.d/note/note
```

## Usage

`todo.txt-note` supports 3 primary functions: `add`, `show` and `edit` (or use their aliases `a`, `s` and `e` respectively)

### Add a Description to a Task

> **IMPORTANT**: `add` a new note into a task which already has a note will effectively ovewrite the existing one. The content of the previous one, however, is kept intact.

```bash
# Simply a single text
$ todo.sh note add 1 -n "The project is defined to be TOP secret"

# Use heredoc
# Please note the second argument, the content of description, MUST be - so that the add-on knows to read from stdin
$ todo.sh note add 1 -n - <<__DOC__
# Description

The project is defined to be TOP secret
__DOC__

# Or a file.
# Please note the `-` is being used to tell `todo.txt-note` to read the content
# from the stdin.
$ cat some_prepared_text.md | todo.sh note add 1 -n -


# By omitting the note's content argument, `todo.txt-note` will use
# `TODO_NOTE_EDITOR` for inputing the content.
$ todo.sh note add 1
# vim is opened
```

## Show the Note

```bash
$ todo.sh note show 1
The project is defined to be TOP secret
```

## Edit the Note Using an External Editor

You can specify your preferred editor either through an environment variable `EDITOR` or `TODO_NOTE_EDITOR`.

> NOTE:
> The default editor is `vi`.
> The order of lookup is to search for `EDITOR` then `TODO_NOTE_EDITOR`.

For example:

```bash
# Your ~/.todo.cfg

# Use Visual Studio Code
export TODO_NOTE_EDITOR=/usr/local/bin/code
```

```bash
$ todo.sh note edit 1
# Use Code to open the file ~/.todo/notes/abcd012.txt
```

## Development

Please have a look at [Creating Add-ons: Examples](https://github.com/todotxt/todo.txt-cli/wiki/Creating-Add-ons%3A-Examples).

You need Python 3.6 or later.
There is no external dependencies.
