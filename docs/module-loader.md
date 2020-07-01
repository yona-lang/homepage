---
title: "Module Loader"
---

Yatta's module loader is very simple and respects environmental variable `YATTA_PATH`. If this variable is not set, then current working directory is used as a base directory for loading modules. Multiple paths may be separated with a default platform path separator: `:` on UNIX and `;` on Windows.

Modules are loaded in the order of directories specified in the `YATTA_PATH`, and first found location wins.

Modules are cached in Yatta, and if a module was already loaded once, then changes made to it on a disk will not be reflected.
