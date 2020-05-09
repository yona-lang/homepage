---
title: "Installation"
draft: false
---

## Installation Instructions
It is possible to run Yatta locally, whether for play purposes or development of new features.

### Build requirements - Debian
    sudo apt install build-essential zlib1g zlib1g-dev 

### Getting GraalVM
    export JAVA_HOME=$HOME/jdk PATH=$JAVA_HOME/bin:$PATH
    wget -O $HOME/jdk.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.0.0/graalvm-ce-java11-linux-amd64-20.0.0.tar.gz
    mkdir $HOME/jdk && tar -xzf $HOME/jdk.tar.gz -C $HOME/jdk --strip-components=1
    $HOME/jdk/bin/gu install native-image
    mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V

### Running Yatta
After cloning the repository, Yatta interpreter can be run simply by:
    ./yatta <filename.yatta>

Note: for simple testing, Yatta can run without file argument, then it will read any input provided to it. It can be ended by Ctrl-D (EOF).
