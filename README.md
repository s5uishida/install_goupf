# Install go-upf on Host
This briefly describes the steps and configuration to directly build and install free5GC UPF ([go-upf](https://github.com/free5gc/go-upf)).
Although [free5GC site](https://free5gc.org/guide/3-install-free5gc/) also has installation instructions, I would like to write down the steps to install go-upf alone.

The specification of the VM that have been confirmed to work is as follows.
| OS | CPU (Min) | Mem (Min) | HDD (Min) |
| --- | --- | --- | --- |
| Ubuntu 24.04 | 1 | 1GB | 10GB |

---

### [Sample Configurations and Miscellaneous for Mobile Network](https://github.com/s5uishida/sample_config_misc_for_mobile_network)

---

<a id="toc"></a>

## Table of Contents

- [Install required packages](#install_packages)
- [Build 5G GTP-U kernel module (gtp5g) and install](#build_gtp5g)
- [Install Golang and Setting](#install_golang)
- [Clone go-upf and build](#build)
- [Get configuration file of upf](#get_config)
- [Run go-upf](#run)
- [Changelog (summary)](#changelog)

---

<a id="install_packages"></a>

## Install required packages

```
# apt -y install git gcc g++ cmake autoconf libtool pkg-config libmnl-dev libyaml-dev
```

<a id="build_gtp5g"></a>

## Build 5G GTP-U kernel module (gtp5g) and install

**Note. If you specify the version with the branch, don't forget to specify it with `-b` option of `git` command.**
```
# git clone https://github.com/free5gc/gtp5g.git
# cd gtp5g
# make
# make install
```
If you want to uninstall it, do the following:
```
# cd gtp5g
# make uninstall
```

<a id="install_golang"></a>

## Install Golang and Setting

```
# wget https://go.dev/dl/go1.23.4.linux-amd64.tar.gz
# tar -C /usr/local -zxvf go1.23.4.linux-amd64.tar.gz
# mkdir -p ~/go/{bin,pkg,src}
# echo 'export GOPATH=$HOME/go' >> ~/.bashrc
# echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
# echo 'export PATH=$PATH:$GOPATH/bin:$GOROOT/bin' >> ~/.bashrc
# echo 'export GO111MODULE=auto' >> ~/.bashrc
# source ~/.bashrc
```

<a id="build"></a>

## Clone go-upf and build

Get go-upf and build it. **Note. If you specify the version with the branch, don't forget to specify it with `-b` option of `git` command.**
```
# git clone https://github.com/free5gc/go-upf.git
# cd go-upf
# CGO_ENABLED=0 go build -o upf cmd/main.go
```
Now the `upf` binary was built.
```
# ls upf
upf
```

<a id="get_config"></a>

## Get configuration file of upf

```
# cd go-upf
# wget https://raw.githubusercontent.com/free5gc/free5gc/main/config/upfcfg.yaml
```

<a id="run"></a>

## Run go-upf

Edit `upfcfg.yaml` and run go-upf.
```
# cd go-upf
# ./upf -c upfcfg.yaml
```

<a id="changelog"></a>

## Changelog (summary)

- [2025.01.18] Initial release.
