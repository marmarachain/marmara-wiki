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
sudo apt-get install libgomp1
```
#### Downloading Assets from Releases
Go to [Releases](https://github.com/marmarachain/marmara/releases) and download Marmara linux binaries .zip from Assets and follow the instructions given below.

> Make sure to follow the following instructions on the downloaded directory or any other directory where you moved or copied. 

```sh
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

#### Downloading Files from Releases

Download the .zip file from Releases page under Assets [here](https://github.com/marmarachain/marmara/releases/) to your Windows computer and extract&place the files (komodod.exe & komodo-cli.exe) in a new folder on the Desktop called ```MCL``` or any other location you prefer (remember the location and use that). 

For this guide, we are using ```MCL``` directory on Desktop.

#### Create the Directory for the Zcash Parameters
Open the command line prompt and execute the following command:

```
mkdir “%HOMEPATH%\AppData\Roaming\ZcashParams”
```

Download following files and move them into the newly created directory i.e. MCL.

- [sprout-proving.key](https://z.cash/downloads/sprout-proving.key)

- [sprout-verifying.key](https://z.cash/downloads/sprout-verifying.key)

- [sapling-spend.params](https://z.cash/downloads/sapling-spend.params)

- [sapling-output.params](https://z.cash/downloads/sapling-output.params)

- [sprout-groth16.params](https://z.cash/downloads/sprout-groth16.params)

## Start and Run the Marmara Chain

To start the chain, open the command prompt and execute the following sets of commands:
```
cd \Desktop\MCL\
komodod.exe -ac_name=MCL -ac_supply=2000000 -ac_cc=2 -addnode=37.148.210.158 -addnode=37.148.212.36 -addressindex=1 -spentindex=1 -ac_marmara=1 -ac_staked=75 -ac_reward=3000000000
```
To stop the chain:
```
cd \Desktop\MCL\
komodo-cli.exe -ac_name=MCL stop
```

#### Backup Wallet

Backing up the **wallet.dat** file is **very critical**. One method to backup this file is to create a copy and archive it.

On Windows machine, the file is located below: 
```
%HOMEPATH%\AppData\Roaming\Komodo\MCL\wallet.dat
```

## Making Marmara Credit Loops
Start the chain as with pubkey as instructed in the usage guideline [here](https://github.com/marmarachain/marmara/wiki/Getting-Started-with-Marmara)

