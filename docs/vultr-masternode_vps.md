# Slick masternode VPS setup for all your beloved crypto masternodes (Vultr example)


---
**PLEASE ALSO READ THE README**

Please also see the [README for this project](../README.md) that will give you a high-level overview of features. 

---

## Intro

This project started as handy bash script to setup my $PIVX masternodes in 2016 when there was almost zero documentation and anything that existed was either $DASH specific, sucked and in most cases both. For that reason, i started to work on a not-so-sucking way to install a lot of different masternodes with next to none manual intervention.

If you are not already aware, visit the project site and join the slack. The website at [https://pivx.org/](https://pivx.org/) is also well worth a visit. 

<img src="images/masternode_vps/intro.png" alt="introduction" class="inline"/>

## Get a VPS system for your masternode(s)

I will use vultr for my instructions, but in practice and with a bit of tuning any hoster that gives you multiple free IPv6 addresses. Register / login with vultr.

Feel free to use my reflink to signup and receive a bonus w/ vultr:
<a href="https://www.vultr.com/?ref=7424168"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

If you wish to use DigitalOcean you can use this referal link but the following guide will only go over Vultr.
Deploy your next app in seconds. Get $100 in cloud credits from DigitalOcean using my link: <a href="https://m.do.co/t/a3f23a15f9af">"<img src="https://blog.microdreamit.com/wp-content/uploads/2019/08/Digital-Ocean-Credit.gif" width="468" height="60"></a>

It's also great that you can use Bitcoin to pay!

<img src="images/masternode_vps/get-a-vps-system-for-your-masternode-s-.png" alt="VPS signup" class="inline"/>

## Deploy a new system

First, create a new VPS by clicking that small "+" button.

<img src="images/masternode_vps/deploy-a-new-system.png" alt="VPS creation" class="inline"/>

## Location choice

The location doesn't matter too much. If in doubt, choose a location next to you.

<img src="images/masternode_vps/location-choice.png" alt="VPS location choice" class="inline"/>

## Linux distribution (Ubuntu 16.04 LTS)

Select Ubuntu 16.04, i am mostly testing for that version.

<img src="images/masternode_vps/linux-distribution--ubuntu-1604-lts-.png" alt="VPS location choice" class="inline"/>

## VPS size

A decent masternode needs a bit of RAM and some storage space. The $5 instance is good enough for up to 5 masternodes. I recommend not running more than 3 production masternodes in parallel, since block rewards suffer from instability (eg when your nodes go down every couple of hours).

<img src="images/masternode_vps/vps-size.png" alt="VPS sizing" class="inline"/>

## Activating additional features (IPv6)

Multiple masternodes on one VPS require multiple IPv6 addresses. Toggle "Enable IPv6" to activate that feature for free (Vultr).

<img src="images/masternode_vps/activating-additional-features--ipv6-.png" alt="VPS sizing" class="inline"/>


## Hostnames & number of VPS

Choose how many instances you want and click "Deploy Now".

<img src="images/masternode_vps/hostnames--amp--number-of-vps.png" alt="VPS sizing" class="inline"/>

## Accessing your VPS via SSH

Copy access credentials for SSH access by opening the server details.

<img src="images/masternode_vps/accessing-your-vps-via-ssh.png" alt="VPS sizing" class="inline"/>

## First SSH session

Login to your newly installed node as "root".

<img src="images/masternode_vps/first-ssh-session.png" alt="VPS sizing" class="inline"/>

## Masternode script installation

Clone this git repository first:

```
git clone https://github.com/MotoAcidic/Abet-MultiNode.git && cd vps
```


## Install the desired masternode and amount

Use the *./install.sh* script with the desired crypto and masternode count as parameters, e.g. to install 4 PIVX masternodes:

```
./install.sh -p abet -c 4
```

The script downloads, compiles and configures the system now. This will usually take between 5-15 minutes.

<img src="images/masternode_vps/install-the-desired-masternode-and-amount.png" alt="VPS sizing" class="inline"/>

The *./install.sh* script outputs a list of possible parameters if executed without options.

## End of installation

The script will output lots of boring stuff and it's ascii banner when done. Your only real work begins now.

<img src="images/masternode_vps/end-of-installation.png" alt="VPS sizing" class="inline"/>


## Masternode configuration files

The generated configuration files are located at /etc/masternodes/. One file per masternode and crypto.

<img src="images/masternode_vps/masternode-configuration-files.png" alt="VPS sizing" class="inline"/>


## Insert your masternode private key

In 99% you can use the generated settings as is. The only value you MUST change is the masternode private key, generated in your controller wallet. Contact the individual crypto community if unsure, although the steps are identical for most master node coins. Check the [Dash documentation for example](https://dashpay.atlassian.net/wiki/spaces/DOC/pages/1867877/Start+multiple+masternodes+from+one+wallet+start-many).

<img src="images/masternode_vps/insert-your-masternode-private-key.png" alt="the master node private key" class="inline"/>


## Start your new masternodes

A script to enable masternode start at boot has been created at */usr/local/bin/activate_masternodes_${CODENAME}.sh* for your convenience. There is exactly one script per installed masternode crypto.

Run it after you finished configuration, e.g. after a Abet installation do.

```
/usr/local/bin/activate_masternodes_abet
```     

## Last step, the controller

To activate the new nodes in your _local_ (not the VPS) controller wallet, add the bind address entries with port to a file called "masternode.conf" as usual.

     MN1 [2002:470:1111:1a4:51]:8322 KEY TX OUTPUT
     MN2 [2003:470:1111:1a4:52]:8322 KEY TX OUTPUT
     MN3 [2003:470:1111:1a4:53]:8322 KEY TX OUTPUT

To make this a bit easier for large installations, i implemented a small gimmick in the newest version. Now after the script has run, a partial of the "masternode.conf" file is generated and placed on the VPS eg for XIOS at "/tmp/abet_masternode.conf"

So you can take the contents from there and paste it into your local controller-wallets masternode.conf all that you need to add is the relevant pieces from "masternode outputs"

<img src="images/masternode_vps/controller_conf_partial.png" alt="controller conference generated partial" class="inline"/>

You get the idea, another step to a fully automated setup... ;-)

## Troubleshooting the masternode on the VPS

If you want to check the status of your masternode, the best way is currently running the cli e.g. via

```
/usr/local/bin/abet-cli -conf=/etc/masternodes/abet_n1.conf getinfo

{
  "version": 3040000,
  "protocolversion": 70917,
  "services": "NETWORK/BLOOM/",
  "walletversion": 61000,
  "balance": 0.00000000,
  "zerocoinbalance": 0.00000000,
  "blocks": 9252,
  "timeoffset": 0,
  "connections": 129,
  "proxy": "",
  "difficulty": 22974.76067187333,
  "testnet": false,
  "moneysupply": 2007131.13233737,
  "keypoololdest": 1576539264,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "staking status": "Staking Not Active",
  "errors": ""
}

```


# Issues and Questions

Please open github issue in case of questions or issues. I might not be able to reply immediately, but i do usually within a couple of days at worst.

If my scripts work for you, please send some crypto my way here:

**Have fun, this is crypto after all!**

Send BTC to:

```
BTC  19U8Jgyb38XnbGyQq3SHXS614pmLbvwKeZ
```
