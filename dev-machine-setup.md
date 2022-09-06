# Development machine setup

## Linux OS

### SSH Key setup

Add public key to gihub account

* Generate ssh key 
```
ssh-keygen -t rsa
```

* Add contents of `.ssh/id_rsa.pub` to Github account

### Configure git on dev machine

* git config --global user.name "user name"
* git config --global user.email "user email"
* Add these two lines to `~/.gitconfig`
```
[url "git@github.com:"]
	insteadOf = https://github.com/
```

### Install Golang

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

#### Shortcut to install lastest Golang

```
wget "https://go.dev/dl/$(curl https://go.dev/VERSION?m=text).linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "$(curl https://go.dev/VERSION?m=text).linux-amd64.tar.gz"
```
