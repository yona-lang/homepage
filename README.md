Using MkDocs: https://www.mkdocs.org/user-guide/writing-your-docs/

# Clone repo

    git clone --recurse-submodules git@github.com:yona-lang/homepage.git

# Installing dependencies

    pip install mkdocs mkdocs-material mkdocs-minify-plugin mkdocs-git-revision-date-localized-plugin mkdocs-awesome-pages-plugin

# Running local development server

    mkdocs serve

Note: if you use a Python virtual environment for development, you might hit errors such as:

> cannot find module 'materialx.emoji' (No module named 'materialx')

If you do, make sure that the python version used to run ``mkdocs serve`` is the same you used to run ``pip install``. Try:

    python3 -m mkdocs serve


# Building site

    mkdocs build
    # commit and push "site/" submodule

# References for plugins used

Awesome Pages:

    https://squidfunk.github.io/mkdocs-material/plugins/awesome-pages/
