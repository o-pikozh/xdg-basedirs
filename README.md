xdg-basedirs
=====================

[XDG Base Directory Specification](http://standards.freedesktop.org/basedir-spec/basedir-spec-latest.html) defines places where applications/scipts/etc should put configs, cache and data. Specifically, it introduces the `$XDG_DATA_HOME`, `$XDG_CONFIG_HOME`, `$XDG_DATA_DIRS`, `$XDG_CONFIG_DIRS`, `$XDG_CACHE_HOME` and `$XDG_RUNTIME_DIR` environment variables, which (when they're set) should to be used by applications/scripts/etc (instead of hard-coding values like `~/.local/share`, `~/.config`, `/usr/local/share:/usr/share`, `/etc/xdg`, `~/.cache`, `/run/user/<user-id>`, which are typical but not obligatory). However, the _XDG Base Directory Specification_ doesn't say that application/script/etc, you're writing, is allowed to expect that all of these variables are initially set -- it instead states default values that must be used when corresponding variable is unset/empty.

This repository provides two very simple (public domain) helper scripts, which hopefully will save you few minutes if you're writing a shell script that works with `~/.config`, `~/.cache`, `~/.local/share` or `/run/user/<uid>` directories. Just copy-paste (one or both of) these helper scripts at the beginning of your shell script, or include them using dot command, e.g.:

    . xdg-basedirs
    . xdg-runtime-dir

Please note, that as purpose of these helper scripts is to set shell variables (XDG_XXX), it is absolutely useless to execute them in usual way (e.g. `./xdg-basedirs` or `sh xdg-runtime-dir`), because neither environment variables, nor shell variables set by child process are visible by the parent process.

So:

1. The **[xdg-basedirs](xdg-basedirs)** sets `$XDG_DATA_HOME`, `$XDG_CONFIG_HOME`, `$XDG_DATA_DIRS`, `$XDG_CONFIG_DIRS` and `$XDG_CACHE_HOME` variables (i.e. all except `$XDG_RUNTIME_DIR`) -- if they're unset/empty -- to values specified by the XDG Base Directory Specification as defaults.

    Note, that it doesn't check whether the directories really exist (so that it's your duty to do something like `mkdir -p "$XDG_CONFIG_HOME"`, or better `mkdir -p "$XDG_CONFIG_HOME/your-app-name"`).

2. The **[xdg-runtime-dir](xdg-runtime-dir)** sets `$XDG_RUNTIME_DIR` variable -- if it is empty/unset. Unlike all other variables from XDG Base Directory Specification, `$XDG_RUNTIME_DIR` is non-trivial to fill: XDG Base Directory Specification doesn't provide exact default value for it, but instead specifies several very specific requirements. Although xdg-runtime-dir cannot satisfy all these requirements (e.g. purge on user logout), it tries to gracefully handle at least some (e.g. permissions, purge on shutdown, warning message) -- so it can be used as fallback mechanism for systems that may provide `$XDG_RUNTIME_DIR` themselves.

Both of these helper scripts set _shell variables_, not _environment variables_ -- i.e. they don't do `export` -- so newly-set values affect only your script, not the applications/scripts/etc that your script will execute.

For any questions you can connect me via the GitHub.
