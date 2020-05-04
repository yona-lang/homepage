# Installation
First install [hugo](https://gohugo.io/). Then:

    mkdir themes
    git submodule add https://github.com/athul/archie.git themes/archie
    git submodule add -b master git@github.com:yatta-lang/yatta-lang.github.io.git public


# Dev server

    hugo server -D


# Build docs

    hugo

Go to public, commit and push to origin. Then also commit source changes to this repo.