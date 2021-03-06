# Deviant Staking Guide
## QT wallet
### Prerequisites
1. The wallet has been setup (verify [here](/common/Setup_wallet.md))
### Creating a Staking Address
Open the Receiving addresses menu from your wallet.<br />

![receiving address](/images/rec-address.png)

Click the "New" push button, fill in the label field with a name, and Confirm with the "OK" push button.
The results will look like the image below (STAKE label as example):

![staking address](/images/receiving-stake.png)

### Enabling Coin Control<br />
The Coin Control feature allows the user to select the input that must be used to withdraw DEV when sending transactions. Without Coin Control enabled, you can't select the sending input, so you may corrupt the setup of input stakes, or destroy an unlocked Masternode collateral, for example. The Send menu, without Coin Control, looks like the below picture:
<br />
![No Coin Control](/images/noCoinControl.png)
<br />
The option to enable the Coin Control feature is in the GUI Wallet. You can activate it by hitting<br />
Settings -> Options -> Wallet<br />

Settings Options | Wallet tab
---------------- | ----------
![GUI-options](/images/GUI-options.png) | ![flagCoinControl](/images/flagCoinControl.png)

<br />
In the wallet tab, just flag "Enable Coin Control" to enable such feature.
Once enabled, the Send menu will look like this:<br />

![box coin control](/images/boxCoinControl.png)

Just click on Coin Control, which will open a new menu.

![flagCoinControl](/images/intoCoinControl2.png)

In this menu, just select the input(s) you want to stake, then press Ok.
### Split your DEV into stake inputs
Copy the amount visible in "After Fee" field in "Amount" field.<br />
Set the previously created address in "Pay To:" field. <br />
Set flag in "Split UTXO" field, then write in the editable field the number (an integer) of inputs you want split your amount.

![UTXO](/images/utxo.png)

Note: There is no formula to calculate the optimal size of inputs. It depends on the values staking on the network. Also, optimal size is not a constant and it changes with time.<br />
Sharing experience about staking with other users may improve the staking experience.
### Start staking
Once the inputs setup is finished, in order to stake you have two prerequisites:
1. Your inputs needs 60 confirmations<br />

A way to count confirmations, is to open "Coin Control" in "list mode", then read confirmations in the homonimous column.

![confirmations](/images/confirmations.png)

2. Your wallet needs to be unlocked (recommended option is "For anonymization, automint, and staking only")<br />

Step | On screen
---- | ---------
Open the menu | ![menu unlock](/images/unlock-wallet-menu.png)
Enter passhphrase | ![enter passphrase](/images/unlock-wallet-password.png)
Verify Lock/Unlock | ![verify unlock](/images/unlock-wallet-verify.png)
Verify Staking status | ![verify staking](/images/staking-icon.png)

## CLI wallet on linux
### Prerequisites
1. The wallet has been setup (verify [here](/common/Setup_wallet.md#setup-cli-wallet-linux))
### Creating a Staking Address
Create a new address with deviant-cli utility:<br />
```deviant-cli getnewaddress "STAKE"```<br />
The address will be shown in the standard output.<br />
![address stake](/images/cli-address-stake.png)

If you need to check it, you can use the command:<br />
```deviant-cli getaddressesbyaccount "STAKE"```<br />
You will send your DEVs to STAKE account address. Keep always in mind that zDEV minting is enabled by default in CLI wallet. If you don't want to mix DEV into zDEV, add the parameter `enablezeromint=0` in file `deviant.conf`.<br />
for example with command:<br />
```echo "enablezeromint=0" >> $HOME/.DeviantCore/deviant.conf```<br />
Or with your preferred text editor.
Once funds have been transferred check them with:<br />
```deviant-cli getbalance``` <br />
To check confirmations number and other details you can fire:<br />
```deviant-cli listunspent``` <br />
![listunspent](/images/cli-wallet-unspent1.png)
### Split your DEV into stake inputs
This step is optional, in fact there is no proof that splitting helps the stakes. It is a practice for believers, says the most.  
There are several ways to split the original input.
The proper way to split UTXO (Unspent Transaction Output) is working with raw transactions.
This way will not be discussed here:<br />
1. This one, is a basic guide<br />
2. The risk to lose coins is high if you have no experience with raw transactions.<br />

There are other ways to split your balance in inputs:<br />
1. Send your DEVs using several transactions (each transaction is an input)
2. Using a change address:<br />
2.1. Unlock wallet for 10 minutes ` deviant-cli walletpassphrase '<your passphrase>' 600` <br />
2.2. Move first input ```deviant-cli sendfrom "ACCOUNT" "toaddress" amount``` <br />
2.3. Move other inputs (one at time), after you have identified your "change addess" with "listunspent", set an account.
```deviant-cli setaccount "address" "account"```
```deviant-cli sendfrom "ACCOUNT" "toaddress" amount```

Example:

Step | Example
---- | -------
2.1 | ![wallet unlock](/images/cli-wallet-unlock.png)
2.2 | ![sendfrom1](/images/cli-sendfrom-account.png)
2.3 | ![listunspent](/images/cli-list-unspent2.png)
2.3 | ![setaccount](/images/cli-set-account.png)
2.3 | ![sendfrom2](/images/cli-send-others.png)

There is no formula to calculate the optimal size of inputs. It depends on the values staking on the network. Also. optimal size is not a constant and it changes with time.<br />
Sharing experience about staking with other users may improve the staking experience.
### Start staking
Unlock the wallet for staking:<br />
```deviant-cli walletpassphrase '<your password>' 0 true``` <br />
Check staking status with:<br />
```deviant-cli getstakingstatus```


