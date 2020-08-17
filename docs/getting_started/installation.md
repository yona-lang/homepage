---
title: "Installation"
---

## Running as a Docker container:
The simplest way to use Yona is to run it in a Docker container. Please see the instructions at [Docker Hub](https://hub.docker.com/r/akovari/yona).

## IDEA Plugin
Set up IDEA [Plugin](https://plugins.jetbrains.com/plugin/14536-yona-language) for Yona, which provides syntax highlighting and syntax validation.

## Running within local installation of GraalVM

### Requirements
* Any OS capable of running [GraalVM](https://www.graalvm.org/getting-started/)
* GraalVM **20.1.0**
* Ensure `JAVA_HOME` environment variable points to the root of your GraalVM installation
* Ensure `PATH` contains your GraalVM root folder + `/bin`

### Installation Instructions
It is possible to run Yona locally, whether for play purposes or development of new features.

### Install GraalVM
These instructions set up GraalVM, required environment variables and install the Yona component into GraalVM.

```bash
export JAVA_HOME=$HOME/jdk PATH=$JAVA_HOME/bin:$JAVA_HOME/graalvm/bin:$PATH
wget -O $HOME/jdk.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.1.0/graalvm-ce-java11-linux-amd64-20.1.0.tar.gz
mkdir $JAVA_HOME && tar -xzf $HOME/jdk.tar.gz -C $JAVA_HOME --strip-components=1
gu install native-image
gu install -ur https://github.com/yona-lang/yona/releases/latest/download/yona-component.jar
```

You can see Yona releases at GitHub [releases](https://github.com/yona-lang/yona/releases) page.

### Run yona interpreter
Yona comes as a GraalVM component, with two executables:
* `yona` - default interpreter, supporting all GraalVM features, such as Polyglot usage, etc.
* `yonanative` - ahead-of-time compiled interpreter that offers faster startup, though may be slower executing longer running programs. Note that `yonanative` currently works only on 64-bit Linux.

Both executables currently accept only a filename as an argument. This file is then executed using the interpreter.
Optionally, if no file is provided, the interpreter will expect an input on the standard input when ran. Ctrl-D marks the end of the input in this case.

!!! tip
    There are many tests available in the Yona [repository](https://github.com/yona-lang/yona/tree/master/language/tests) that can serve as a good starting point for writing some programs in Yona.

### Interpreter logging
Logging can be enabled to debug certain types of issues, for example related to module loading. To do so, make sure your interpreter(either `yona` or `yonanative`) is ran with:
```bash
yona --log.yona.yona.runtime.Context.level=CONFIG
```

or additionally you can try increasing the log level all the way down to `FINE`, `FINER` or even `FINEST`.

## Local development setup
These instructions are for setting up a local development environment. Currently Debian 10 is the only verified platform (though, any Linux that runs GraalVM should in theory work).

### Build requirements - Debian
```bash
sudo apt install build-essential zlib1g zlib1g-dev 
```

### Download and install Maven
Download Maven from their [site](https://maven.apache.org/download.cgi). Then unpack it somewhere and make sure maven/bin path is added to `$PATH`.

### Getting GraalVM
```bash
mvn package -DskipTests=true -Dmaven.javadoc.skip=true -B -V
```

### Running Yona
After cloning the repository, Yona interpreter can be run simply by:
```bash
./yona <filename.yona>
```
