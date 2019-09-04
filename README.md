# Nodemaster

The **Nodemaster** scripts is a collection of utilities to manage, setup and update masternode instances.

I am quite confident this is the single best and almost effortless way to setup different crypto masternodes, without bothering too much about the setup part.

If this script helped you in any way, please contribute some feedback. BTC donations also welcome and never forget:

**Have fun, this is crypto after all!**

```
BTC  1JfM1MU6Ro8e9VBiYzSTvYbzJUyLiupcNr
```


Feel free to use my reflink to signup and receive a bonus w/ vultr:
<a href="https://www.vultr.com/?ref=6903922"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

## Supported masternode projects

The ever growing list of supported projects is now maintained at [https://github.com/CryptoCashBack-Hub/Advanced-Install/tree/master/config](https://github.com/CryptoCashBack-Hub/Advanced-Install/tree/master/config).

---

**NOTE on the VPS choice for starters**

**Vultr** is highly recommended for this kind of setup. I created an [easy step-by-step guide for the VPS provider vultr](/docs/masternode_vps.md) that will guide you through the hardest parts.

---

## About / Background

A very in depth guide for beginners (https://steemit.com/masternodes/@swcrypto/ccbc-easy-guide-to-setup-multiple-masternodes-on-1-vps) 

Many masternode crypto currencies only have incomplete or even non-existing instructions available how to setup a masternode from source.

This project started as handy bash script to setup my $PIVX masternodes in 2016 when there was almost zero documentation and anything that existed was either $DASH specific, sucked and in most cases both. For that reason, i started to work on a not-so-sucking way to install a lot of different masternodes with next to none manual intervention.

If you are not already aware, visit the project site and join the slack. The website at [https://pivx.org/](https://pivx.org/) is also well worth a visit.

Many people use binaries, end of with an insecure configuration or fail completely. This is obviously bad for the stability of the individual network.

After doing hundreds of masternode installations in the past two years, i decided to share some of my existing auto-install and management scripts with the community to work on a generalised & reliable setup for all masternode coins.

Comparing with building from source manually, you will benefit from using this script in the following way(s):

* 100% auto-compilation and 99% of configuration on the masternode side of things. It is currently only tested on a vultr VPS but should work almost anywhere where IPv6 addresses are available
* Developed with recent Ubuntu versions in mind, currently only 16.04 is supported
* Installs 1-100 (or more!) masternodes in parallel on one machine, with individual config and data
* Compilation is currently from source for the desired git repo tag (configurable via config files)
  Some security hardening is done, including firewalling and a separate user
* Automatic startup for all masternode daemons
* This script needs to run as root, the masternodes will and should not!
* It's ipv6 enabled, tor/onion will follow

## Installation

SSH to your VPS and clone the Github repository:

```bash
git clone https://github.com/CryptoCashBack-Hub/Advanced-Install.git && cd Advanced-Install
```

Install & configure your desired master node with options:

```bash
./install.sh -p ccbc
```

## Examples for typical script invocation

These are only a couple of examples for typical setups. Check my [easy step-by-step guide for [vultr](/docs/masternode_vps.md) that will guide you through the hardest parts.

**Install & configure 4 CCBC masternodes:**

```bash
./install.sh -p ccbc -c 4
```

**Update daemon of previously installed CCBC masternodes:**

```bash
./install.sh -p ccbc -u -n "6"
```

**Install 6 CCBC masternodes with the git release tag "tags/v1.0.0.5"**

```bash
./install.sh -p ccbc -c 6 -r "tags/v1.0.0.5"
```

**Wipe all CCBC masternode data:**

```bash
./install.sh -p ccbc -w
```

**Install 2 CCBC masternodes and configure sentinel monitoring:**

```bash
./install.sh -p ccbc -c 2 -s
```

## Options

The _install.sh_ script support the following parameters:

| Long Option  | Short Option | Values              | description                                                         |
| :----------- | :----------- | ------------------- | ------------------------------------------------------------------- |
| --project    | -p           | project, e.g. "ccbc"| shortname for the project                                           |
| --net        | -n           | "4" / "6"           | ip type for masternode. (ipv)6 is default                           |
| --release    | -r           | e.g. "tags/v1.0.0.2"| a specific git tag/branch, defaults to latest tested                |
| --count      | -c           | number              | amount of masternodes to be configured                              |
| --update     | -u           | --                  | update specified masternode daemon, combine with -p flag            |
| --sentinel   | -s           | --                  | install and configure sentinel for node monitoring                  |
| --wipe       | -w           | --                  | uninstall & wipe all related master node data, combine with -p flag |
| --help       | -h           | --                  | print help info                                                     |
| --startnodes | -x           | --                  | starts masternode(s) after installation                             |

## Troubleshooting the masternode on the VPS

If you want to check the status of your masternode, the best way is currently running the cli e.g. for $MUE via

```
/usr/local/bin/ccbc-cli -conf=/etc/masternodes/ccbc_n1.conf getinfo

{
    "version" : 1000004,
    "protocolversion" : 70004,
    "walletversion" : 61000,
    "balance" : 0.00000000,
    "zerocoinbalance" : 0.00000000,
    "blocks" : 15719,
    "timeoffset" : 0,
    "connections" : 21,
    "proxy" : "",
    "difficulty" : 47075.67091216,
    "testnet" : false,
    "moneysupply" : 8015522.58694497,
    "zCCBCsupply" : {
        "1" : 1.00000000,
        "5" : 5.00000000,
        "10" : 10.00000000,
        "50" : 50.00000000,
        "100" : 100.00000000,
        "500" : 0.00000000,
        "1000" : 0.00000000,
        "5000" : 0.00000000,
        "total" : 166.00000000
    },
    "keypoololdest" : 1537990754,
    "keypoolsize" : 1001,
    "paytxfee" : 0.00000000,
    "relayfee" : 0.00010000,
    "staking status" : "Staking Not Active",
    "errors" : ""
}
```
# Helpful Commands

## Start Coin on initial install
```
/usr/local/bin/activate_masternodes_ccbc
```
## Stop coin
```
/usr/local/bin/ccbc-cli -conf=/etc/masternodes/ccbc_n1.conf stop
```
## Start Coin
```
/usr/local/bin/ccbcd -conf=/etc/masternodes/ccbc_n1.conf
```
## Getinfo
```
/usr/local/bin/ccbc-cli -conf=/etc/masternodes/ccbc_n1.conf getinfo
```


# Help, Issues and Questions

I activated the "[issues](https://github.com/masternodes/vps/issues)" option on github to give you a way to document defects and feature wishes. Feel free top [open issues](https://github.com/masternodes/vps/issues) for problems / features you are missing here: [https://github.com/masternodes/vps/issues](https://github.com/masternodes/vps/issues).

I might not be able to reply immediately, but i do usually within a couple of days at worst. I will also happily take any pull requests that make masternode installations easier for everyone ;-)

If this script helped you in any way, please contribute some feedback. BTC donations also welcome and never forget:

**Have fun, this is crypto after all!**

```
BTC  1JfM1MU6Ro8e9VBiYzSTvYbzJUyLiupcNr
```

## Management script (not yet implemented)

The management script release will follow within the next couple of days.

| command                               | description                                  |
| :------------------------------------ | -------------------------------------------- |
| nodemaster start ccbc (all\|number)   | start all or a specific pivx masternode(s)   |
| nodemaster restart ccbc (all\|number) | stop all or a specific pivx masternode(s)    |
| nodemaster stop ccbc (all\|number)    | restart all or a specific pivx masternode(s) |
| nodemaster cleanup ccbc (all\|number) | delete chain data for all pivx masternodes   |
| nodemaster status ccbc (all\|number)  | systemd process status for a pivx masternode |
| nodemaster tail ccbc (all\|number)    | tail debug logs for a pivx masternode        |

# Todo

* provide my Dockerfile & Vagrantfile
* write more test cases
* implement a binary option (?)
* output all supported cryptos as list within help

# Errors

* currently not fully idempotent

Ping me at contact@marsmenschen.com for questions and send some crypto my way if you are happy.

**Have fun, this is crypto after all!**

```
BTC  1JfM1MU6Ro8e9VBiYzSTvYbzJUyLiupcNr
```
