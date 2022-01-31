# How to Use pipenv

The ```pipenv``` tool lets you establish custom environments for each python project so that the dependencies aren't all mashed together globally. It is a similar idea to maven with Java or node_modules with javascript, but rather different to work with.

## Set up

Create a folder to work in.

```bash
$ mkdir -p ~/vscode/poc/envtest
$ cd ~/vscode/poc/envtest
```
Make sure your global version of python is set to 3.9.6 as indicated by the asterisk.
```bash
$ pyenv versions

  system
  3.6.15
* 3.9.6 (set by /Users/georgecampbell/.pyenv/version)
```

If 3.6.15 or 3.9.6 are missing, install them:

```bash
$ pyenv install 3.6.15
$ pyenv install 3.9.6
```

Now you can select 3.9.6 to work with.

```bash
$ pyenv global 3.9.6
$ python --version
Python 3.9.6
```

Let's try installing a dependency globally. "requests" is a commonly used library that makes http requests.

```bash
$ pip install requests
$ pip show requests
Name: requests
Version: 2.27.1
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: /Users/georgecampbell/.pyenv/versions/3.9.6/lib/python3.9/site-packages
Requires: certifi, charset-normalizer, idna, urllib3
Required-by: 
```
Look carefully at the location. See how global dependencies are always stored in the folder associated with the current version of python. Let's switch over to 3.6.15 and see if requests is still installed.

```bash
$ pyenv global 3.6.15
$ pip show requests
```

Nothing - it is not installed for Python version 3.6.15. Switch back to 3.9.6 and remove requests from the global space:

```bash
$ pyenv global 3.9.6
$ pip uninstall requests
Uninstalling requests-2.27.1:
  Would remove:
    /Users/georgecampbell/.pyenv/versions/3.9.6/lib/python3.9/site-packages/requests-2.27.1.dist-info/*
    /Users/georgecampbell/.pyenv/versions/3.9.6/lib/python3.9/site-packages/requests/*
Proceed (Y/n)? Y
  Successfully uninstalled requests-2.27.1

$ pip show requests

```

So to summarize:
* Global python dependencies are stored in a folder associated with the global python version.
* Installing a dependency in one version does not install it in another.
* This is a problem if we want different dependencies for different projects, typically at ```/Users/georgecampbell/.pyenv/versions``` on the mac.

## Project Specific Environments

So we should still be in the ```~/vscode/poc/envtest``` folder.

```bash
$ pwd
/Users/georgecampbell/vscode/poc/envtest
```

Let's create an environment here and install ```numpy```, an array mathematics depenency. For this project we want to use Python 3.6.15 even though we have globally set 3.9.6. We will shell into the environment we create and you can see the python version change.

```bash
$ pipenv install --python 3.6.15
$ python --version
Python 3.9.6

$ pipenv shell
$ python --version
Python 3.6.15

$ pip -V
pip 21.3.1 from /Users/georgecampbell/.local/share/virtualenvs/envtest-_yx9iYIl/lib/python3.6/site-packages/pip (python 3.6)

$ exit
$ python --version
Python 3.9.6

$ pip -V
pip 21.3.1 from /Users/georgecampbell/.pyenv/versions/3.9.6/lib/python3.9/site-packages/pip (python 3.9)
```
You can actually set your mac prompt to show if you are in a python environment or not.

See [How to configure your macos terminal with zshell like a pro](https://www.freecodecamp.org/news/how-to-configure-your-macos-terminal-with-zsh-like-a-pro-c0ab3f3c1156/) and use these prompt settings in your ```.zshrc``` file.

```bash
plugins=(
    macos
    zsh-syntax-highlighting
    colorize
    colored-man-pages
    virtualenv
)
source $ZSH/oh-my-zsh.sh

if [[ "$(arch)" == "arm64"  ]]; then
  source /opt/homebrew/opt/powerlevel9k/powerlevel9k.zsh-theme
else
  source usr/local/opt/powerlevel9k/powerlevel9k.zsh-theme
fi
setopt AUTO_CD AUTO_PUSHD EXTENDED_GLOB
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(dir)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(virtualenv status)
```

In the last line, see how I display the ```virtualenv status```? It looks like this in my terminal, with the environment displayed on the right side when I'm shelled in.

```bash
 ~/vscode/poc/envtest >                                envtest-_yx9iYIl <
```

## Install numpy in your project environment

Make sure you are shelled into your environment. You can tell with

```bash
$ pip -V
pip 21.3.1 from /Users/georgecampbell/.local/share/virtualenvs/envtest-_yx9iYIl/lib/python3.6/site-packages/pip (python 3.6)
```

Now lets install ```numpy```. Note that we use ```pipenv```, not ```pip```, to install dependencies. Note that if you are on Mac with an M1 arm64 processor, you need to start up your terminal in X86_64 mode using Rosetta 2 because numpy won't compile on an M1 CPU (or at least not without doing a lot of research on how).

```bash
$ pipenv install numpy
$ pipenv show numpy
georgecampbell@Georges-MacBook-Air envtest % pip show numpy
Name: numpy
Version: 1.19.5
Summary: NumPy is the fundamental package for array computing with Python.
Home-page: https://www.numpy.org
Author: Travis E. Oliphant et al.
Author-email:
License: BSD
Location: /Users/georgecampbell/.local/share/virtualenvs/envtest-_yx9iYIl/lib/python3.6/site-packages
Requires:
Required-by:
```
This is very interesting. The numpy dependency is installed in a custom folder that only this project environment has access to:

```~/.local/share/virtualenvs/envtest-_yx9iYIl/lib/python3.6/site-packages```

So what if you want to package this project up so it can be installed inside a docker container? The dependency information is stored in ```pipfile.lock``` so that when the project is downloaded from git and ```pipenv install``` is run on it, the dependencies will install.

```bash
$ cat pipfile.lock
{
    "_meta": {
        "hash": {
            "sha256": "bf07c3f1de3ebe8a1c5fe2eae2d98da8a6d40d5eb706b01882175bce834fccdf"
        },
        "pipfile-spec": 6,
        "requires": {
            "python_version": "3.6"
        },
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.org/simple",
                "verify_ssl": true
            }
        ]
    },
    "default": {
        "numpy": {
            "hashes": [
                "sha256:012426a41bc9ab63bb158635aecccc7610e3eff5d31d1eb43bc099debc979d94",
                ... etc, etc, etc
            ],
            "index": "pypi",
            "version": "==1.19.5"
        }
    },
    "develop": {}
}
```
Another possible way of extracting the dependencies is to create a requirements.txt file

```bash
$ pip freeze > requirements.txt
$ cat requirements.txt
numpy==1.19.5
```

If someone downloads the project and runs ```pip install``` it will check the requirements.txt file. This is the traditional way of noting the dependencies for python projects. Pipenv is relatively new, but is nice to work with.

## Summary

* Global dependencies are stored in the folder for the selected global python version, and those dependencies are available to all python versions.

```bash
Users/georgecampbell/.pyenv/versions/{X.Y.Z}/lib/python{X.Y}/site-packages/
```

* Pip environment depencies are stored in a special location by environment name, and dependencies from one project are only available to that project.

```bash
/Users/georgecampbell/.local/share/virtualenvs/envtest-_yx9iYIl/lib/
```

where ```envtest-_yx9iYIl``` is the environment name created when ```pipenv install``` was run in the empty ```envtest``` folder.

* The ```pipfile.lock``` file and ```requirements.txt``` files keep track of the projects dependencies. The ```pipfile.lock``` gets updated automatically every time you add a dependency with ```pipenv install {NAME}``` while the requirements.txt file must be created / updated manually with ```pip freeze > requirements.txt```