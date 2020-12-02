## Building Marmara Credit Loops Smart Chain From Source

One may also build Marmara Credit Loops Smart Chain from source. This is not required, however building from source is considered as the best practice in a production environment, since this allows one to instantly update to the latest patches and upgrades.

### Linux

#### Requirements

    Linux (easiest with a Debian-based distribution, such as Ubuntu)
        For Ubuntu, we recommend using the 16.04 or 18.04 releases

    64-bit Processor

    Minimum 2 CPUs

    Minimum 4GB of free RAM

#### Get Started
Verify that your system is up to date.
```	
sudo apt-get update
sudo apt-get upgrade -y
```

#### Install the dependency packages

```	
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libsodium-dev libdb++-dev ntp ntpdate software-properties-common curl clang libcurl4-gnutls-dev cmake clang -y
```
This action takes some time, depending on your Internet connection. Let the process run in the background.

Once completed, follow the steps below to install Marmara Credit Loops smart chain.

#### Clone the Marmara Credit Loops Repository
```	
cd ~
git clone https://github.com/marmarachain/marmara komodo --branch master --single-branch
```
>Skip the following steps and jump to **__Fetch the Zcash Parameters__** step if the system you are using has 8 GB or more RAM.

#### Changing swap size to 4 GB
Swap is a space on a disk which is used when the amount of physical RAM memory is full. 
If the system has 4 GB of RAM, to avoid cases of running out of RAM, swap space needs to be configured.

>Check if OS already has swap enabled using ``` sudo swapon --show ```. 
If swapfile exists and is not configured as at least 4 GB then turn off the swap before configuring it by ```sudo swapoff /swapfile ```

Execute the following commands to allocate 4 GB of RAM for the swap space. 
```
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile 
sudo mkswap /swapfile 
sudo swapon /swapfile
```
#### Fetch the Zcash Parameters
```
cd komodo
./zcutil/fetch-params.sh
./zcutil/build.sh -j$(nproc)
```
**The following steps are optional to follow.**

##### Setting up Swappiness
Swappiness is the kernel parameter that defines how much (and how often) Linux kernel copies RAM contents to swap. 
The following sets up the parameter's value as “10”. Note that the higher the value of the swappiness parameter, the more aggressively Linux's kernel will swap.
```
sudo sysctl vm.swappiness=10 
```
>This setting will persist until the next reboot. We can set this value automatically at restart by adding the line to our /etc/sysctl.conf file:
>```sudo nano /etc/sysctl.conf``` vm.swappiness=10

#### Enabling UFW
This step is not required for installing MCL but can be used for server security purposes.

- Check the status of UFW by
```
sudo ufw status
```
- UFW package can be installed by executing the following command
```	
sudo apt install ufw
```
- Activate UFW and allow connections by executing the following commands
```
echo y | sudo ufw enable
sudo ufw allow ssh
sudo ufw allow "OpenSSH"
sudo ufw allow 33824
sudo ufw allow out 33824
```


### macOS

#### Requirements

- OSX (version > 10.11)
- Minimum 4GB of free RAM (8GB+ recommended)

<br>
<b>Ensure Command Line Tools are Installed</b>

Issue the following command in a terminal.
```
xcode-select –install
```
<b>Ensure brew is Installed</b>

We use the software brew to install dependencies. If you have the latest version of brew installed already, you may skip this step.
```
usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew install/master/install)"
```

<b>Use brew to Install Dependencies</b>

Execute each command separately
```
brew update
brew upgrade
brew tap discoteq/discoteq; brew install flock
brew install autoconf autogen automake
brew install gcc@8
brew install binutils
brew install protobuf
brew install coreutils
brew install wget
```

<b>Clone the Marmara Credit Loops Repository</b>

```
cd ~
git clone https://github.com/marmarachain/marmara komodo --branch master --single-branch
```

<b>Fetch and Compile the Zcash Parameters</b>
```
cd komodo
./zcutil/fetch-params.sh
./zcutil/build-mac.sh -j8
```

<b>Create Configuration File</b>

Create the configuration file in the following directory:
```
~/Library/Application\ Support/Komodo
```

If the directory does not yet exist, create the directory.

```
mkdir ~/Library/Application\ Support/Komodo
```

Create the configuration file by entering the following commands in the terminal. Execute each line separately.

```
echo "rpcuser=komodouser" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "rpcpassword=`head -c 32 /dev/urandom | base64`" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "txindex=1" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "bind=127.0.0.1" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "rpcbind=127.0.0.1" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=5.9.102.210" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=78.47.196.146" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=178.63.69.164" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=88.198.65.74" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=5.9.122.241" >> ~/Library/Application\ Support/Komodo/komodo.conf
echo "addnode=144.76.94.38" >> ~/Library/Application\ Support/Komodo/komodo.conf
```


Run Komodo
once all processes are complete, run the komodod daemon.
```
cd ~/komodo/src
./komodod &
```


Using komodo-cli and getinfo

```
cd ~/komodo/src
./komodo-cli getinfo
```

When the returned properties of blocks and longestchain are equal to each other, the daemon is finished syncing with the network.

<b>Backup Your Wallet</b>

On MacOS, the file is located here: 
```
~/Library/Application\ Support/Komodo/wallet.dat
```

One method to backup this file is to archive a copy of the file.

Copy the file
```
cp -av ~/Library/Application\ Support/Komodo/wallet.dat ~/wallet.dat
```
Rename file
```
mv ~/wallet.dat ~/2019-05-17-wallet_backup.dat
```

To make archive
```
tar -czvf ~/2019-05-17-wallet_backup.dat.tgz ~/2019-05-17-wallet_backup.dat
```
Move the final file to a secure location



Having completed these, launch the Marmara Chain by following the instructions in [here](https://github.com/marmarachain/marmara/wiki/Marmara-Credit-Loops).