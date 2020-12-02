# Debian K3d Demo

Simple debian virtual machine to run k3d locally.

Note, this installs the latest `k3d`, `kubectl`, `helm`, `k9s`, `kubectx`, `skaffold` and `kustomize` packages.

# Requirements

 - [Download](https://git-scm.com/downloads) and install GIT
 - [Download](https://www.virtualbox.org/wiki/Downloads) and install Virtualbox
 - [Download](https://www.vagrantup.com/downloads.html) and install Vagrant

# Startup

- Clone the repo:

```
$ git clone https://github.com/sdaoudi/debian-k3d-demo.git
```

- Start Vagrant

```
$ cd debian-k3d-demo
$ vagrant up
```

- SSH Access:

```
$ vargrant ssh
```

- Create your cluster:

```
$ k3d cluster create demo --servers 1 --agents 2
```
