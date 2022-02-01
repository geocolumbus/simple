# Simple Python Data Analysis Deploy

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
#### Install python versions

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
#### Install pipenv
 
pipenv allows you to isolate the environments of each project.

See [how to use pipenv](./README-pipenv.md).

```bash
$ brew install pipenv
$ pipenv --version
pipenv, version 2022.1.8
```
## Usage

### Download and Install

To use this project, clone it from Github and install the python dependencies.

```bash
$ mkdir ~/vscode/poc
$ cd ~/vscode/poc
$ git clone https://github.com/geocolumbus/simple.git
$ cd simple
$ pipenv install
```
### Setup VS Code

Start VS Code and open the ```~/vscode/poc/simple``` folder.

Tell VS Code to use the environment we created with the ```pipenv install``` command.

1. Press ⇧⌘P to display the Command Palette.

1. Type ```Python: Sel``` to call up the ```Python: Select Interpreter``` command. You will see the various environments. Choose the one for this project, ```simple-{hash code}```. Note that it points to the pipenv folder at ```~/.local/share/virtualenvs/simple-{hash code}/bin/python```
1. Look at the lower left hand of the IDE - you should see ```Python 3.6.15 ...``` displayed.

### Configure DataRobot credentials

We want to access DataRobot python APIs on our local machine. Later we will embed these credentials in a key store so we can upload our application to the cloud.

1. Go to [https://www.datarobot.com/](https://www.datarobot.com/) and choose "Start for Free"

1. Sign in with your credentials. If you don't have credentials, sign up for a free account.
1. Under your profile picture at the top right of the page, select ```Developer Tools```
1. Create a new key and give it a name. Copy it to the clipboard.
1. Store it here:
```
~
└── .config
    └── datarobot
        └── drconfig.yaml
```
Where drconfig.yaml looks like this:
```
token: NjFkYjZm...
endpoint: https://app2.datarobot.com/api/v2
```

### Configure AWS credentials

We need AWS credentials so we can create AWS infrastructure in our AWS account to run our application.

You obtain credentials by accessing the AWS Console and selecting the IAM Service. You create an Access Key ID and Secret Access Key, and possibly a Session Token, which you store in the credentials folder. You will have to assign roles to that user to allow them to create cloud infrastructure .... more on that later.

```
~
├── .aws
│   ├── config
│   ├── credentials
│   └── credentials.bak
```
#### AWS Roles - TODO
