# ULEAD Masternode VPS Installation

This masternode installation script vastly simplifies the setup of a ulead masternode running on a virtual private server (VPS), and it also adds a number of other powerful features, including:

* IPv6 Support
* Installs 1-100 (or more!) ulead masternodes in parallel on one VPS, with individual ulead.conf and data directories
* It can install masternodes for other coins on the same VPS as ulead
* 100% auto-compilation and 99% of configuration on the masternode side of things
* Automatically compiling from the latest ulead release tag, or another tag can be specified
* Automatic startup for all masternode daemons

Some notes and requirements:

* Script has only been tested on a Vultr VPS, but should work almost anywhere where IPv6 addresses are available
* Currently only Ubunto 16.04 Linux is supported
* This script needs to run as root or with sudo, the masternodes will and should not!

This project was forked from Phore project. Phore project was forked from https://github.com/masternodes/vps. @marsmensch (Florian) is the primary author behind this VPS installation script for masternodes. If you would like to donate to him, you can use the BTC address below

**Have fun, this is crypto after all!**

```
BTC  33ENWZ9RCYBG7nv6ac8KxBUSuQX64Hx3x3
```

# Install guide on vultr

## How to get VPS server

For new masternode owners, **Vultr** is recommended as a VPS hosting provider, but other providers that allow direct root SSH login access and offer Ubunto 16.04 may work.

You can use the following referral link to sign up with Vultr for VPS hosting:

<a href="https://www.vultr.com/?ref=7302091"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

## Deploy a new system

First, create a new VPS by clicking that small "+" button.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/deploy-a-new-system.png" alt="New VPS" class="inline"/>

## Location choice

You can choose any location. You may wish to have it hosted in a city/country near you, or choose a different area to help with the global decentralization of the ULEAD masternode network.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/location-choice.png" alt="VPS location choice" class="inline"/>

## Linux distribution (Ubuntu 16.04 LTS)

Select Ubuntu 16.04.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/linux-distribution--ubuntu-1604-lts-.png" alt="VPS location choice" class="inline"/>

## VPS size

The 25 GB SSD / 1024MBB Memory instance is enough for 4-5 masternodes. You may need more memory as the ulead blockchain grows over time, or if you want to run more masternodes.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/vps-size.png" alt="VPS sizing" class="inline"/>

## Activating additional features (IPv6)

Toggle "Enable IPv6" to activate that feature--at Vultr there is no additional cost for this.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/activating-additional-features--ipv6-.png" alt="VPS sizing" class="inline"/>

You may wish to enable DDOS Protection to protect your masternodes against a potential denial of service attack, especially if you are running multiple masternodes from one VPS. Vultr charges an additional fee for this.

## Hostnames & number of VPS

Choose how many instances you want and click "Deploy Now".

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/hostnames--amp--number-of-vps.png" alt="VPS sizing" class="inline"/>

## Installation of PuTTY as SSH client (Windows)
If you are running your wallet from Windows, install PuTTY while the server is being set up. You can download PuTTY from here: http://www.putty.org/. Skip this step if you are using a Mac--you will use the built in Terminal application instead.

Once PuTTY is installed, return to the Vultr dashboard to get the login details by clicking on the ... to the right of your server, and select Server Details.

## Accessing your VPS via SSH

Copy your password for SSH access from the server details page.
<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/accessing-your-vps-via-ssh.png" alt="check hostname and password" class="inline"/>

Now open PuTTY to add the server.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/login-to-vps-via-PuTTY.png" alt="login to VPS" class="inline"/>

Enter the IP address in the Host Name field, and enter the server name you wish to use for this VPS (e.g., MN01) to Saved Sessions. Click save.

Click the open button. When the console has opened, click Yes in the PuTTY Security Alert box.
<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/PuTTY-Security-Alert.png" alt="Alert from PuTTY" class="inline"/>

Now enter your server login details provided in your Vultr account.
You cannot Ctrl+V to paste in the console. Either right click the mouse or type shift+insert (sometimes
on keyboard it will just be INS key)

User: root
Password: (paste or type password)

When you paste it will not display, so don't try to paste again.
Just paste once and press Enter.

For Mac users, open Terminal (e.g., Press Command-Space and type Terminal and press Enter). Then type:

```
ssh -l root <IP address>
```
## Install Masternode

Login to your newly installed node as "root".

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/first-ssh-session.png" alt="VPS sizing" class="inline"/>

Enter this command to copy the Masternode installation script and install a single ulead Masternode:

```bash
git clone https://github.com/uleadapp/vps.git && cd vps && ./install.sh -p ulead
```

If you have your masternode private key, please use this (you can generate masternode private key with Step 2 below).

```bash
git clone https://github.com/uleadproject/vps.git && cd vps && ./install.sh -p ulead -k **PRIVATE KEY**
```
Using this command, you can skip "Configure masternode configuration files" below, because the command abopve adds the masternode private key to the masternode configuration files.

This prepares the system and installs the ulead Masternode daemon. This includes downloading the latest ulead masternode release, creating a swap file, configuring the firewall, and compiling the ulead Masternode from source code. This process takes about 20-25 minutes.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/install-the-desired-masternode-and-amount.png" alt="VPS configuration" class="inline"/>

While that is underway, go back to your local desktop and open ulead-qt.

### More complex situations (ignore if you are installing a single masternode on a new VPS)

If you wish to install more than one masternode on the same VPS, you can add a -c parameter to tell the script how many to configure, so for example this would install three ulead masternodes (all entered on one line):

The below command will install 3 masternodes:
```bash
git clone https://github.com/uleadproject/vps.git && cd vps && ./install.sh -p ulead -c 3
```

If you already have your masternode private keys, you can add them as shown below (all entered on one line):

```bash
git clone https://github.com/uleadproject/vps.git && cd vps && ./install.sh -p ulead -c 3 --key **PRIVATE KEY 01** --key2 **PRIVATE KEY 02** --key3 **PRIVATE KEY 03**
```
Using this command, you can skip the step for "Configure masternode configuration files", because the command above adds the masternode private keys to the masternode configuration files.

## Configure ulead Wallet
### Step1 - Create Collateral Transaction
Once the wallet is open on your local computer, generate a new receive address and label it however you want to identify your masternode rewards (e.g., ulead-MN-01). This label will show up in your transactions each time you receive a block reward.

Click the Request payment button, and copy the address.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/step1-newaddress.png" alt="making new address" class="inline"/>

Now go to the Send tab, paste the copied address, and send *exactly* 1,500,000 ulead to it in a single transaction. Wait for it to confirm on the blockchain. This is the collateral transaction that will be locked and paired with your new masternode. If you are setting up more than one masternode at one time, repeat this process for each one.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/step1-send10kphr.png" alt="sending 10kPHR" class="inline"/>

### Step 2 - Generate Masternode Private Key
Go to the **[Tools > Debug Console]** and enter these commands below:

```bash
masternode genkey
```
This will produce a masternode private key:

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/step2-masternodegenkey.png" alt="generating masternode private key" class="inline"/>

Copy this value to a text file. It will be needed for both the ulead configuration file on the masternode VPS, and the masternode configuration file on the computer with the controlling ulead wallet.

If you are setting up multiple masternodes, repeat this step for each one. Each time you run the masternode genkey command it will give you a new private key--it doesn't matter which one you use, but it is important that it is unique for each masternode and that the VPS ulead configuration file and wallet masternode configuration file match (see below).

### Step 3 - Masternode Outputs

This will give you the rest of the information you need to configure your masternode in your ulead wallet--the transaction ID and the output index.

```bash
masternode outputs
```

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/step3-masternodeoutputs.png" alt="getting transaction id" class="inline"/>

The long string of characters is the *Transaction ID* for your masternode collateral transaction. The number after the long string is the *Index*. Copy and paste these into the text file next to the private key you generated in Step 2.

If you have multiple masternodes in the same wallet and have done the 1,500,000 ulead transactions for each of them, masternode outputs will display transaction IDs and indexes for each one. You can choose which private key to go with each transaction ID and index, as long as they are all different, and you make sure the corresponding lines in masternode.conf and the VPS ulead configuration files match (see below).

## End of installations
When the script finishes, it will look similar to this:

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/end-of-installation.png" alt="installation ended" class="inline"/>

You only have a few steps remaining to complete your masternode configuration.
## Configure masternode configuration files
Since this installation method supports multiple masternodes, the ulead configuration files have a node number added to them (e.g., ulead_n1.conf, ulead_n2.conf), stored in the /etc/masternodes directory. If you have a single masternode on the VPS, you will only need to edit /etc/masternodes/ulead_n1.conf.

To open ulead_n1.conf for editing, enter these commands:
```bash
sudo apt-get install nano
nano /etc/masternodes/ulead_n1.conf
```
The next step adds your masternode private key.

## Add masternode private key
What you need to change is only masternode private key.
(We recommend using IPv6 which is the default, but if you choose IPv4 when you ran the installation script, please edit #NEW_IPv4_ADDRESS_FOR_MASTERNODE_NUMBER to your VPS IP address).
After typing the nano command, you will see something similar to this.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/insert-your-masternode-private-key.png" alt="add private key" class="inline"/>

Copy the masternode private key from the text file you saved it in, and replace HERE_GOES_YOUR_MASTERNODE_KEY_FOR_MASTERNODE_ulead_1 with that private key (this typically begins with an 8).

While you have this file opened, copy the information that follows after masternodeaddr=, starting with the open bracket. This is the masternode's IPv6 address and port, and will be needed for the wallet's masternode.conf file.

Once you have your masternode private key entered, press <font color="Green">Ctrl+X</font> .
Then press <font color="Green">Y</font> to save, and press Enter to exit.

Finally, close and restart your ulead wallet so that it will have the new masternode configuration.

## Start your masternodes
A script for starting all masternodes on the VPS has been created at /usr/local/bin/activate_masternodes_ulead.sh.
Run this command after your masternode configuration written above.

```bash
/usr/local/bin/activate_masternodes_ulead
```

The masternode daemons will start and begin loading the ulead blockchain.

## Finishing Wallet Configuration & Activate Masternode
To activate your nodes from your wallet, one of the last steps is to add a line for the masternode in the masternode.conf file. This file has the following format, with each value separated with a space:

* alias IP:Port masternodeprivatekey collateral_transaction_ID collateral_output_index
* alias - A short name you use to identify the masternode, you can choose this name as long as it is without spaces (e.g., ulead-MN-01)
* IP:Port - The IP address (either IPv6 or IPv4) and the Port where the masternode is running, separated by a colon (:). You copied this from the ulead.conf file on the VPS.
* collateral_transaction_ID: This is the transaction ID you copied from masternode outputs.
* collateral_output_index: This is the index you copied from masternode outputs.

From the wallet menu, edit the local wallet **masternode.conf** file. **[Tools > Open Masternode Configuration File]**
Add the MN conf line, like the example below to the masternode.conf file. Save it, and close the file. It will look like the following example, using your values for each of the fields above. A common mistake is mixing up the private key and the collateral transaction ID--to make this easier, the private key usually begins with an 8.

example.
```
ulead-MN-01 [2001:19f0:5001:ca6:2085::1]:11788 88xrxxxxxxxxxxxxxxxxxxxxxxx7K 6b4c9xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx7ee23 0
```

The image below shows another example using an IPv4 IP address. If you followed this guide you are probably using an IPv6 address that looks like the line above.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/masternode-conf.png" alt="editing masternode.conf" class="inline"/>

If you are running multiple masternodes, you need to add one of these lines for each masternode, and make sure the private key on each line matches the corresponding private key you entered in the VPS ulead configuration file for that masternode.
## Check syncing status of masternode
The masternode cannot complete activation until it is fully synced with the ulead blockchain network.

To check the status of your masternode, please enter this command in the VPS terminal. If you have multiple masternodes on the same VPS, you can change n1 to n2 etc. below to check the status of each one.

```bash
/usr/local/bin/ulead-cli -conf=/etc/masternodes/ulead_n1.conf getinfo
```
The output will look like this:
```
{
  "version": 1050000,
  "protocolversion": 70005,
  "walletversion": 61000,
  "balance": 425323688.32143605,
  "zerocoinbalance": 0.00000000,
  "blocks": 21964,
  "timeoffset": 0,
  "connections": 124,
  "proxy": "",
  "difficulty": 22225487.17525773,
  "testnet": false,
  "moneysupply": 2559152142.67340985,
  "zuleadsupply": {
    "1": 0.00000000,
    "5": 0.00000000,
    "10": 0.00000000,
    "50": 0.00000000,
    "100": 0.00000000,
    "500": 0.00000000,
    "1000": 0.00000000,
    "5000": 0.00000000,
    "total": 0.00000000
  },
  "keypoololdest": 1546565199,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "staking status": "Staking Active",
  "errors": ""
}

```

We're looking at the *blocks*, and need that to be the latest block in the blockchain. You can check your local wallet to see the latest block by hovering over the green check mark.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/check-blocks-number.png" alt="checking syncing status" class="inline"/>

Once your masternode has synced up to the latest block, go to next step. The syncing process may take 15-30 minutes or more as the ulead blockchain grows. You can keep checking progress with the command above, by pressing the up arrow and Enter to repeat it.

## Start Masternode

Go to the debug console of your ulead wallet **[Tools->Debug Console]** and enter the following command, replacing **mn-alias** with the name of the masternode in the Alias column of the Masternodes tab:

```
startmasternode alias false mn-alias
```

You may need to unlock the wallet **[Settings->Unlock Wallet]** before you run this command, entering your passphrase. You can lock the wallet after it is finished.

If everything was setup correctly, after entering the command you will see something like this:
```
{
"overall" : "Successfully started 1 masternodes, failed to start 0, total 1",
"detail" : {
"status" : {
"alias" : "ulead-MN-01",
"result" : "successful"
}
```
If you are setting up multiple masternodes, repeat this for each one. You can now close the debug console, return the Masternodes tab and check the status:
<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/check-masternode-status.png" alt="checking syncing status" class="inline"/>

It should say ENABLED, and within an hour, the timer in the Active column should start increasing.

Your ulead masternode is now set up and running! Depending on how many masternodes there are, it may take 12-24 hours before you see your first masternode reward--this is normal and rewards should come at more regular intervals after the first one.

<img src="https://github.com/uleadapp/vps/blob/master/docs/images/masternode_vps/rewards.png" alt="rewards" class="inline"/>

## Issues and Questions
Please open a GitHub Issue if there are problems with this installation method. Many ulead team members activel support people installing masternodes and can provide assistance in the ulead Discord channel.
Here is a Discord invitation:

https://discord.gg/RkdQ8C

If you would like to make a donation to ulead's ongoing development, you can send ulead to the core team at this address: UYBM3EsRaSn2NoUazaZHeJ4FJ9BKs2dKSD
