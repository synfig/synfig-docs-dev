Synfig developer documentation
==============================

Project documentation is built using [Sphinx docs](http://sphinx-doc.org/), which uses [ReST](http://docutils.sourceforge.net/rst.html) for markup.  This allows the docs to cover a vast amount of topics without using a thousand-line README file.

We are using [pipenv](https://docs.pipenv.org/) to install Sphinx docs.

Installation:
```bash
$ sudo pip3 install pipenv
$ cd docs
$ pipenv install --three
```

This will install the requirements needed for the generating the docs. Afterwards you can run:

```bash
$ make html
```

The docs will be generated, the output files will be placed in the `_build/html/` directory, and can be browsed (locally) with any browser.

The docs can also be found online at <https://synfig-dev-docs.readthedocs.org/>.