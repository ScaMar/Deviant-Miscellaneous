# Workaround for Hot Masternode on windows
## Issue
You are running a Hot Masternoden on Windows Server 2012 and/or Windows Server 2016.
The Masternode status always reach status "MISSING".
The command `masternode status` executed in debug console, always give this outputs:
```Masternode not found in list of available masternodes. Current status: Not capable masternode:```<br />
You have double checked your configuration, everything seems Ok, simply your Masternode refuse to go online.
## Workaround
As a workaround you have several options,
any of them is a variant of Hot / Cold masternode setup.
In this guide you will be driven to setup a Hot / Cold Masternode on the very same Windows Server.
## Hot wallet (GUI wallet)
### Case 1: You already have a running deviant-qt wallet
Probably, because you are checking this workaround, you already installed the wallet.
In this case, close your wallet. Rename your deviant.conf in deviant.conf.bak. Create a new deviant.conf with only a line:<br />
```
listen=0
rpcport=22619
```

You can do this in the way you like, here an example via command line. <br />
```
cd %APPDATA%\DeviantCore
move deviant.conf deviant.conf.bak
echo rpcport=22619 > deviant.conf
echo listen=0 >> deviant.conf
```
![Adj deviant.conf](/images/WIN-adj-deviant.conf.png)

If your setup match this case, you already did the [preparation steps](/common/Preparation-steps-for-MN.md)

Restart your wallet
### Case 2: You need to install the wallet
In this case follow the [guide](/common/Setup_wallet.md). I suggest to use the standalone executables, uncompress them in folder C:\Deviantcoin.<br />

![Suggested location](/images/WIN-sugg-loc.png)

Fill your deviant.conf with only one line:
```
listen=0
rpcport=22619
```

![deviant.conf-workaround](/images/WIN-two-lines.png)

Start you wallet
## Cold wallet (CLI wallet)
As Cold wallet we will use the `deviantd.exe` that is shipped within wallet package. To setup the deamon without beeing in conflict with the wallet follow these steps.

1. Create a new data directory
With cmd prompt:<br />
```mkdir %APPDATA%\DeviantCoreMN```

With explorer:<br />
![New datadir](/images/WIN-new-datadir.png)

2. Create the management scripts directory
![scripts dir](/images/WIN-dir-scripts.png)

3. Create the management scripts:<br />
3.1. Start daemon script
Open the notepad, paste these lines in it:<br />
```
C:\Deviantcoin\dev-3.0.0.1-win64\deviantd.exe -conf=%APPDATA%\DeviantCoreMN\deviant.conf -datadir=%APPDATA%\DeviantCoreMN
```
![start script](/images/WIN-start-script.png)

Save as "Dev-MN.bat" in folder `C:\Deviantcoin\scripts`
Note: select "All files" in "Save as type:", otherwise it will be saved as "txt" with hidden extension.

![Save as Dev-MN](/images/WIN-dev-mn.bat.png)

3.2 deviant-cli script
Open the notepad, paste these lines in it:<br />
```
C:\Deviantcoin\dev-3.0.0.1-win64\deviant-cli.exe -conf=%APPDATA%\DeviantCoreMN\deviant.conf -datadir=%APPDATA%\DeviantCoreMN %*
```
![cli script](/images/WIN-cli-script.png)

Save as "deviant-cli.bat" in folder `C:\windows\system32`
Note: select "All files" in "Save as type:", otherwise it will be saved as "txt" with hidden extension.

![Save as cli](/images/WIN-cli.bat.png)

4. Fill deviant.conf
Open notepad, fill with info for you system using this template:<br />
```
masternode=1
masternodeprivkey=<MASTERNODE PRIV KEY> 
bind=<PUBLIC IP>
externalip=<PUBLIC IP>:22618
rpcallow=127.0.0.1
```
In this guide the following `deviant.conf` has been used:<br />

![deviantd deviant.conf](/images/WIN-daemon-conf.png)

5. Start the Cold wallet
Browse the directory until you reach `C:\Deviantcoin\scripts`, then execute `Dev-MN.bat`

![start daemon](/images/WIN-start-daemon.png)

This action will open a window, do not close it, otherwise you will kill the daemon

![daemon window](/images/WIN-daemon-window.png)

6. Check the sync process
At this step, the daemon is running, you need to wait for full sync before start the masternode. You can check the sync status with following command:<br />
```deviant-cli.bat mnsync status```

![cli mnsync](/images/WIN-cli-status.png)

You can proceed forward when this line appear in the output:
`"IsBlockchainSynced": true`
## Start the masternode
To start the masternode open the GUI wallet, then select the "Masternodes" tab.
Use "Start Missing" function. Insert the password when needed.

![start missing](/images/WIN-start-missing.png)

## Check masternode status
To check if the start is really succesful you can use this command:<br />
```deviant-cli.bat masternode status```

![masternode status](/images/WIN-mn-status.png)







































































