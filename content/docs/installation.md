---
title: "Installation"
---

## Running as a Docker container:
The simplest way to use Yatta is to run it in a Docker container. Please see the instructions at [Docker Hub](https://hub.docker.com/r/akovari/yatta).

## Running within local installation of GraalVM

### Requirements
* Any OS capable of running [GraalVM](https://www.graalvm.org/getting-started/)
* GraalVM **20.1.0**
* Ensure `JAVA_HOME` environment variable points to the root of your GraalVM installation
* Ensure `PATH` contains your GraalVM root folder + `/bin`

### Installation Instructions
It is possible to run Yatta locally, whether for play purposes or development of new features.

### Install GraalVM
These instructions set up GraalVM, required environment variables and install the Yatta component into GraalVM.

```bash
export JAVA_HOME=$HOME/jdk PATH=$JAVA_HOME/bin:$JAVA_HOME/graalvm/bin:$PATH
wget -O $HOME/jdk.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.1.0/graalvm-ce-java11-linux-amd64-20.1.0.tar.gz
mkdir $JAVA_HOME && tar -xzf $HOME/jdk.tar.gz -C $JAVA_HOME --strip-components=1
gu install native-image
gu install -ur https://github.com/yatta-lang/yatta/releases/latest/download/yatta-component.jar
```

You can see Yatta releases at GitHub [releases](https://github.com/yatta-lang/yatta/releases) page.

### Run yatta interpreter
Yatta comes as a GraalVM component, with two executables:
* `yatta` - default interpreter, supporting all GraalVM features, such as Polyglot usage, etc.
* `yattanative` - ahead-of-time compiled interpreter that offers faster startup, though may be slower executing longer running programs. Note that `yattanative` currently works only on 64-bit Linux.

Both executables currently accept only a filename as an argument. This file is then executed using the interpreter.
Optionally, if no file is provided, the interpreter will expect an input on the standard input when ran. Ctrl-D marks the end of the input in this case.

### Interpreter logging
Logging can be enabled to debug certain types of issues, for example related to module loading. To do so, make sure your interpreter(either `yatta` or `yattanative`) is ran with:
```bash
yatta --log.yatta.yatta.runtime.Context.level=CONFIG
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

### Running Yatta
After cloning the repository, Yatta interpreter can be run simply by:
```bash
./yatta <filename.yatta>
```
