---
title: "File"
tags: stdlib
---

This module provides functions for reading, writing and otherwise manipulating files.
## Usage

All file access is non-blocking and asynchronous. Behind the courtains, file operations use [`AsynchronousFileChannel`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/channels/AsynchronousFileChannel.html).
Files can be worked in either text or binary mode. Text mode anticipates that file is encoded in UTF-8 encoding, whereas binary file may contain any contents.

### Opening a file
File must be opened before used. Function `open` takes a path and set of file "modes" as arguments and returns a file handle. This handle is actually a tuple with several items, that should not be accessed directly, but instead handled to functions operating on a file, from this module.

Opening file for reading, in a text mode, for example:

```haskell
    fh = File::open "test.txt" {:read}
```

Available modes (for further details, refer to Java's [`StandardOpenOption`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/StandardOpenOption.html)):

| Mode | Description |
| ---- | ----------- |
| `:read` | Open for read access. |
| `:write`| Open for write access. |
| `:append` | If the file is opened for `:write` access then bytes will be written to the end of the file rather than the beginning. |
| `:truncate_existing` | If the file already exists and it is opened for `:write` access, then its length is truncated to 0. |
| `:create` | Create a new file if it does not exist. |
| `:create_new` | Create a new file, failing if the file already exists. |
| `:delete_on_close` | Delete on close. |
| `:sparse` | Sparse file. |
| `:sync` | Requires that every update to the file's content or metadata be written synchronously to the underlying storage device. |
| `:dsync` | Requires that every update to the file's content be written synchronously to the underlying storage device. |

### Closing a file
File should be closed when no longer used. This ensures that file may no longer be used, after calling this function. This function expects a file handle and returns a `()`.

```haskell
    File::close fh
```

### Deleting a file
Function to delete an existing file takes a file handle a returns a `()`.

```haskell
    File::delete fh
```

### Seeking to a position in a file
Seeking to a position takes a file handle, new position (integer) and returns an updated file handle:

```haskell
    new_fh = File::seek fh position
```

### Creating a temporary file
In order to create a temporary file in a system temp folder, pass a prefix (string), suffix (string) and a set of file modes. Function returns a new handle, with a newly created temporary file, opened using the specified file modes.

```haskell
    fh = File::make_temp "prefix" "suffix" {:write}
```

### Obtaining a path from a file handle
It can be useful to obtain a path from a file handle, for example when working with temporary files. Path is a string.

```haskell
    path = File::path fh
```

### Reading file - line mode
File can be read either as a whole, or in lines (provided that it is a text file).

Reading the whole in a line mode can be implemented for example this way:

```haskell
    count_file_lines file = read_lines fh 0

    read_lines fh acc =
        case File::read_line fh of
            (:ok, _, new_fh)  -> read_lines new_fh (acc + 1)
            :eof              -> acc
        end
```

Function `read_all_lines` will count the number of lines in a file.

### Reading file - all at once
To read the whole file, use function `read`. This function returns a sequence containing the contents of the file (either as a string, or sequence of bytes, depending on the mode in which the file is opened).

```haskell
    file_contents = File::read file
```

### Writing to file
Writing to file requires a file to be opened in a `:write` mode. The `write` functin expects a file handle and a sequence (string or a byte sequence) to be written to the file.
The sequence will be written at whatever position the file handle is currently pointing to.

```haskell
    File::write temp_file "hello world"
```
