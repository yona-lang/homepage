---
title: "Installation"
---

## Requirements
* Any OS capable of running GraalVM
* GraalVM **20.0.0**
* Ensure `JAVA_HOME` environment variable points to the root of your GraalVM installation

## Installation Instructions
It is possible to run Yatta locally, whether for play purposes or development of new features.

### Install GraalVM
Follow [Getting Started](https://www.graalvm.org/getting-started/) guide on the GraalVM site or run:

```bash
export JAVA_HOME=$HOME/jdk PATH=$JAVA_HOME/bin:$PATH
wget -O $HOME/jdk.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.0.0/graalvm-ce-java11-linux-amd64-20.0.0.tar.gz
mkdir $HOME/jdk && tar -xzf $HOME/jdk.tar.gz -C $HOME/jdk --strip-components=1
gu install native-image
```

### Setup Yatta Component
Download yatta-component.jar from the [releases](https://github.com/yatta-lang/yatta/releases) section. Then run:

```bash
gu install -L yatta-component.jar
```

### Run yatta interpreter
Yatta comes as a GraalVM component, with two executables:
* `yatta` - default interpreter, supporting all GraalVM features, such as Polyglot usage, etc.
* `yattanative` - ahead-of-time compiled interpreter that offers faster startup, though may be slower executing longer running programs

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
