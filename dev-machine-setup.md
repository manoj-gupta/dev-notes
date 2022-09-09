# Development machine setup (Linux)

# Set up SSH

## Install Essentials

```
manoj@ubuntu:/# apt-get update && apt-get upgrade
manoj@ubuntu:/# apt-get install -y build-essential vim wget git
manoj@ubuntu:/# apt-get install -y sudo pkg-config libz-dev
```

## SSH Key setup

Add public key to gihub account

* Generate ssh key 
```
manoj@ubuntu:/# ssh-keygen -t rsa
```

* Add contents of `.ssh/id_rsa.pub` to [GitHub](https://github.com/settings/keys)

### Configure git on dev machine

```
manoj@ubuntu:~# git config --global user.name "user name"
manoj@ubuntu:~# git config --global user.email "user email"
```

Add these two lines to `~/.gitconfig`
```
[url "git@github.com:"]
	insteadOf = https://github.com/
```

# Install Golang

* Remove existing `golang` installation, if any
```
sudo apt-get remove golang-go
```

or

```
sudo rm -rf /usr/local/go
```

* Get the latest golaong from https://golang.org/doc/install or `wget`
```
wget https://dl.google.com/go/go1.18.1.linux-amd64.tar.gz
```

* untar it to `/usr/local`
```
sudo tar -C /usr/local -xzf go1.18.1.linux-amd64.tar.gz
```

* Add the following to `.bashrc` and restart terminal
```
export GOPATH=$HOME/go
export PATH=$PATH:/usr/sbin:/usr/local/go/bin:$GOPATH/bin
export GOPRIVATE=github.com/goforedge
```

## Shortcut to install lastest Golang

```
wget "https://go.dev/dl/$(curl https://go.dev/VERSION?m=text).linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "$(curl https://go.dev/VERSION?m=text).linux-amd64.tar.gz"
```

# Install ReactJS

* Step1: Install NPM

```
$ sudo apt install npm
$ npm --version
$ node --version
```

*  Step2: Install create-react-app utility

```
$ sudo npm -g install create-react-app
$ create-react-app --version
```

* Step 3: Create & Launch Your First React Application 

```
$ create-react-app first-app
```

# Install Visual Studio Code

* Step 1: Update the packages index and install the dependencies

```
$ sudo apt update
$ sudo apt install software-properties-common apt-transport-https wget
```

* Step 2: Import Microsoft GPG key and enable VS Code repository

```
$ wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
```

* Step 3: Install VS Code

```
$ sudo apt install code
```

**Note:** When a new version is released you can update the Visual Studio Code package through your desktop standard Software Update tool or by running the following commands in your terminal:

```
$ sudo apt update
$ sudo apt upgrade
```

