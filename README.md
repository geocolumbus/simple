# Simple DataRobot Deploy

## Introduction

This project demonstrates a simple Data Robot python project that is deployed to the cloud. The following technologies are demonstrated

* Python
* VS Code
* Git
* Docker
* Ansible
* Terraform
* Data Robot APIs

## Prerequisites

### Install VS Code

https://code.visualstudio.com/

### Install Plugins

* AREPL for python
* Community Material Theme
* Docker
* Jupyter
* Kite AutoComplete AI Code
* Pylance
* Python
* Python Docstring Generator
* Python Test Explorer
* Test Adapter Converter
* Test Explorer UI
* YAML

### Install Command Line tools

#### -- pyenv --

The Python Environment tool helps you manage your python versions.

```bash
$ brew install pyenv
```
Note that you will need this in your mac's ```.zshrc``` file:
```bash
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```
Restart your terminal after installing

```bash
source ~/.zshrc
```

```bash
$ pyenv install 3.6.15
$ pyenv install 3.9.6
$ pyenv global 3.9.6

$ pyenv versions
  system
  3.6.15
* 3.9.6 (set by /Users/georgecampbell/.pyenv/version)

$ python --version
Python 3.9.6
```
#### -- pipenv --
 
pipenv allows you to isolate the environments of each project

```bash
$ brew install pipenv
$ pipenv --version
pipenv, version 2022.1.8
```
Take a moment and read [how to use pipenv](./README-pipenv.md).

