## Installing Marmara Credit Loops from Pre-compiled Binaries

One can download and unzip our pre-compiled binaries. This is the simplest method and hence requires no compiling procedure.

### Linux 

Verify that your system is up to date.

```	
sudo apt-get update
sudo apt-get upgrade -y
```

#### Install the dependency packages

```	
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libsodium-dev libdb++-dev ntp ntpdate software-properties-common curl clang libcurl4-gnutls-dev cmake clang -y
```
#### Downloading Assets from Releases
Go to [Releases](https://github.com/marmarachain/marmara/releases) and download Marmara linux binaries .zip from Assets and follow the instructions given below.

```sh
wget https://github.com/marmarachain/marmara/releases/download/v1.1/MCL-linux.zip
sudo apt install unzip
unzip MCL-linux.zip
sudo chmod +x komodod komodo-cli fetch-params.sh
./fetch-params.sh
```
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
### Windows

- download zip from [here](https://github.com/marmarachain/marmara/releases/download/v1.1/MCL-win.zip)
- unzip files
- run fetch-params.bat

## Start the chain
Start the chain as instructed in the usage guideline [here](https://github.com/marmarachain/marmara/wiki/Getting-Started-with-Marmara)

