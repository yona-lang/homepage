---
title: "Module Loader"
---

Yona's module loader is very simple and respects environmental variable `YONA_PATH`. If this variable is not set, then current working directory is used as a base directory for loading modules. Multiple paths may be separated with a default platform path separator: `:` on UNIX and `;` on Windows.

Modules are loaded in the order of directories specified in the `YONA_PATH`, and first found location wins.

Modules are cached in Yona, and if a module was already loaded once, then changes made to it on a disk will not be reflected.

!!! tip
    It is possible to execute a module (run `yona -f file.yona`) if this file is a module exporting a zero-argument function `main`.
