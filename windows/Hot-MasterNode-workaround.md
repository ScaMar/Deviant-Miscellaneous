# Workaround for Hot Masternode on windows
## Issue
You are running a Hot Masternode on Windows Server 2012 and/or Windows Server 2016.<br />
The Masternode status always reaches status "MISSING".<br />
The command `masternode status` executed in debug console, always gives this output:<br />
```Masternode not found in list of available masternodes. Current status: Not capable masternode:```<br />
You have double checked your configuration, everything seems Ok, yet simply your Masternode refuses to go online.
## Workaround
As a workaround you have several options.
Any of them is a variant of the Hot / Cold masternode setup.
In this guide, you will be driven to setup a Hot / Cold Masternode on the very same Windows Server.
## Cold wallet (GUI wallet)
### Case 1: You already have a running deviant-qt wallet.
Probably, because you are checking this workaround, you already installed the wallet.
In this case, close your wallet. Rename your deviant.conf in deviant.conf.bak. Create a new deviant.conf with only two lines:<br />
```
listen=0
rpcport=22619
```

You can do this in whichever way you like. Here is an example via command line. <br />
```
cd %APPDATA%\DeviantCore
move deviant.conf deviant.conf.bak
echo rpcport=22619 > deviant.conf
echo listen=0 >> deviant.conf
```
![Adj deviant.conf](/images/WIN-adj-deviant.conf.png)

If your setup matches this case, you already did the [preparation steps](/common/Preparation-steps-for-MN.md). Also, you already have a proper `masternode.conf`.


### Case 2: You need to install the wallet.
In this case, follow the [guide](/common/Setup_wallet.md). I suggest using the standalone executables, and uncompressing them in folder C:\Deviantcoin.<br />

![Suggested location](/images/WIN-sugg-loc.png)

Fill your deviant.conf with only two lines:
```
listen=0
rpcport=22619
```

![deviant.conf-workaround](/images/WIN-two-lines.png)

Start your wallet and check [preparation steps](/common/Preparation-steps-for-MN.md) in order to be ready to execute the masternode.<br />
Add your line to file `masternode.conf`:<br />
`{MN ALIAS} {YOUR PUBLIC IP}:22618 {MN PRIV KEY} {MN TX-iD} {TX-ID IDX}`<br />
The values above are explained in the Preparation steps, except `{YOUR PUBLIC IP}`.<br />
You can check your public ip by opening the url: [http://ident.me](http://ident.me).

## Hot wallet (CLI wallet)
With the Hot wallet, we will use the `deviantd.exe` that is shipped within the wallet package. To setup the deamon without being in conflict with the wallet, follow these steps:

1. Create a new data directory<br />
With cmd prompt:<br />
```mkdir %APPDATA%\DeviantCoreMN```

With the explorer:<br />
![New datadir](/images/WIN-new-datadir.png)

2. Create the management scripts directory<br />
![scripts dir](/images/WIN-dir-scripts.png)

3. Create the management scripts:<br />
3.1. Start daemon script<br />
Open the notepad, and paste these lines in it:<br />
```
C:\Deviantcoin\dev-3.0.0.1-win64\deviantd.exe -conf=%APPDATA%\DeviantCoreMN\deviant.conf -datadir=%APPDATA%\DeviantCoreMN
```
![start script](/images/WIN-start-script.png)

Save as "Dev-MN.bat" in folder `C:\Deviantcoin\scripts`
Note: Select "All files" in "Save as type:", otherwise it will be saved as "txt" with a hidden extension.

![Save as Dev-MN](/images/WIN-dev-mn.bat.png)

3.2 deviant-cli script<br />
Open the notepad, and paste these lines in it:<br />
```
C:\Deviantcoin\dev-3.0.0.1-win64\deviant-cli.exe -conf=%APPDATA%\DeviantCoreMN\deviant.conf -datadir=%APPDATA%\DeviantCoreMN %*
```
![cli script](/images/WIN-cli-script.png)

Save as "deviant-cli.bat" in folder `C:\windows\system32`
Note: select "All files" in "Save as type:", otherwise, it will be saved as "txt" with a hidden extension.

![Save as cli](/images/WIN-cli.bat.png)

4. Fill deviant.conf<br />
Open the notepad, and fill it with info for your system using this template:<br />
```
masternode=1
masternodeprivkey={MN PRIV KEY} 
bind={YOUR PUBLIC IP}
externalip={YOUR PUBLIC IP}:22618
rpcallow=127.0.0.1
```
In this guide, the following `deviant.conf` has been used:<br />

![deviantd deviant.conf](/images/WIN-daemon-conf.png)

5. Start the Hot wallet<br />
Browse the directory until you reach `C:\Deviantcoin\scripts`, then execute `Dev-MN.bat`

![start daemon](/images/WIN-start-daemon.png)

This action will open a window. Do not close it, otherwise, you will kill the daemon.

![daemon window](/images/WIN-daemon-window.png)

6. Check the sync process<br />
By this step, the daemon is running. You need to wait for it to fully sync before starting the masternode. You can check the sync status with following command:<br />
```deviant-cli.bat mnsync status```

![cli mnsync](/images/WIN-cli-status.png)

You can proceed forward when this line appears in the output:
`"IsBlockchainSynced": true`
## Start the masternode
To start the masternode, open the GUI wallet, then select the "Masternodes" tab.
Use the "Start Missing" function. Insert the password when needed.

![start missing](/images/WIN-start-missing.png)

## Check masternode status
To check to see if the start is really succesful, you can use this command:<br />
```deviant-cli.bat masternode status```

![masternode status](/images/WIN-mn-status.png)

## Do not forget!
1. Enable [Windows firewall](/windows/windows-firewall.md)
2. Unlock executables (right-click on every deviant* executables in folder C:\Deviantcoin\dev-3.0.0.1-win64, then flag the "unlock" checkbox).

## Troubleshooting the configuration

File / Parameter | Cold - GUI wallet - deviant-qt | Hot - CLI wallet - deviantd
---------------- | ----------------------------- | ----------------------------
Data Directory | `%APPDATA%\DeviantCore` | `%APPDATA%\DeviantCoreMN`
deviant.conf | ![GUI dev.conf](/images/WIN-gui-devconf.png) | ![CLI dev.conf](/images/WIN-cli-devconf.png)
masternode.conf | ![GUI mn.conf](/images/WIN-gui-mnconf-cut.png) | ![CLI mn.conf](/images/WIN-cli-mnconf-cut.png)

Note: The screenshots of masternode.conf files are cutted. You just have to note.:<br />
Cold - GUI wallet - deviant-qt --> filled file [uncutted](/images/WIN-gui-mnconf.png)<br />
Hot - CLI wallet - deviantd --> default file (not filled with your data) [uncutted](/images/WIN-gui-mnconf.png)<br />











































































