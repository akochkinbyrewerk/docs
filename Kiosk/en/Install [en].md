# Trovemat software install guide
There are 2 possible options for running Trovemat software:
1. [Linux Mint Cinnamon x64](https://linuxmint.com/edition.php?id=237) - version for demo purpose only, can be installed with the desktop manager and require minimal experience and knowledge of the Linux OS (or no experience at all). Recommended for first-time users, not familiar with Linux and/or Trovemat software.
1. [Ubuntu 16.04 Server x64](http://releases.ubuntu.com/16.04/ubuntu-16.04.3-server-amd64.iso) - version for using Trovemat software in the real world. Contains all security settings, required for secure running Trovemat software.

## Installing Trovemat software with Linux Mint Cinnamon x64
1. Install Linux Mint Cinnamon x64.
1. Install Trovemat software (by running following command in the OS command shell)  
    
    > !!! ATTENTION  
    > !!! CURRENT COMMAND INSTALLS DEMO-VERSION OF THE TROVEMAT SOFTWARE  
    > !!! FOR THE UNLIMITED VERSION  
    > !!! EMAIL TO sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer.sh -O /tmp/trovemat_online_installer.sh && sudo bash /tmp/trovemat_online_installer.sh $USER
    ```
1. Calibrate touch-screen using the following shell command:
    ``` SHELL 
    sudo apt install xinput-calibrator && xinput_calibrator --device {id}
    ```
    You can find id of the touch-screen (enter without curly braces) after installing Trovemat software in the device list.
1. Restart OS, > Trovemat software will launch automatically after OS starts.
1. Setup Trovemat software according to ["Trovemat software User Guide"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md)

## Installing Trovemat software with Ubuntu 16.04 Server x64
1. Install Ubuntu 16.04 Server x64
    1. Make sure that you check "OpenSSH Server" option while installing OS.
    1. Use default settings while installing OS.
1. Install Trovemat software (by running following command in the OS command shell)  
    
    > !!! ATTENTION  
    > !!! CURRENT COMMAND INSTALLS DEMO-VERSION OF THE TROVEMAT SOFTWARE  
    > !!! FOR THE UNLIMITED VERSION  
    > !!! EMAIL TO sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
    chmod +x trovemat_online_installer_ubuntu.sh
    sudo ./trovemat_online_installer_ubuntu.sh $USER
    ```
    
    1. Installing licensed version of the Trovemat Software
        
        For installing license version of the Trovemat software you need to have an URL for downloading this version. After getting this URL you can run following commands to install that license version:

			wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
			chmod +x trovemat_online_installer_ubuntu.sh
			sudo ./trovemat_online_installer_ubuntu.sh $USER ENTER_URL_OF_THE_LICENSED_VERSION_OF_THE_TROVEMAT_SOFTWARE

			Example with URL https://files.bytewerk.com/files/11111111111/trovemat-latest.tar.xz:
			wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
			chmod +x trovemat_online_installer_ubuntu.sh
			sudo ./trovemat_online_installer_ubuntu.sh $USER https://files.bytewerk.com/files/11111111111/trovemat-latest.tar.xz

1. When install is complete, computer will reboot. After system start Trovemat software will start automatically.
1. Setup Trovemat software according to ["Trovemat software User Guide"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md)

## Typical commands, used for Trovemat software set-up from TOX messenger

You can set-up Trovemat software by sending commands from any application, supporting TOX protocol.
Before sending any commands to Trovemat Kiosk, you need to add your "tox id" to Kiosk's friend list:
1. Enter service menu by pressing sequentially F1 and F2 on the keyboard, attached to Kiosk.
1. Press "Add admin" - first added administrator will be created with the full rights, and also you can't delete that admin anymore using service menu.
1. In TOX client you need to accept request from Trovemat software and wait till Trovemat goes online in the client application's friends list.

Below, there are several main commands, that needs to be run when you setup your Trovemat software for the first time (in that document we give some test data in the commands description just for example). Full description of all available commands can be viewed in document ["Trovemat software User Guide"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md). You can use any quotes in the commands - double quotes, single quotes, and quotes from the smartphone onscreen keyboars etc.

    !!! ATTENTION  
    !!! You have to restart Kiosk's OS after running all set-up commands, using the following command from TOX client: service restart

In the descriptions below we use angle brackets to identify parameters, that needs to be filled up with concrete values, according to Trovemat software installation details. For example, string like "<INTEGER NUMBER - ID OF THE KIOSK>" in text of real command will look like "2".

1. Change Kiosk name:

		settings set "config.parameters.point_name" "<KIOSK NAME>"
1. Change Kiosk ID:

		settings set "config.parameters.point_id" "<INTEGER NUMBER - ID OF THE KIOSK>"
1. Save login and password for Jetcrypto Wallet account:
    1. Login:

			settings set "config.gateways.jetcrypto_wallet->username" "<JetCrypto Wallet login>"
    1. Password:

			settings set -secure "crypto.jetcrypto_wallet_password" "<JetCrypto Wallet password>"
1. Save access keys from chosen cryptocurrency exchange (we using Poloniex account for an example - see the following document for set-up other supported exchanges ["Trovemat software User Guide"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md#settings-for-crytpocurrency-exchange)):
    1. Public key:
    
			settings set "config.payments.common_params.<Parameter name, where public key will be stored>" "<Contents of the public key>"
			
			Example using Poloniex:
			settings set "config.payments.common_params.poloniex_public_key" "00000000-00000000-00000000-00000000"
    1. Private key:
    
			settings set -secure "crypto.<Parameter name, where secret key will be stored>" "<Contents of the secret key>"
			
			Example using Poloniex:
			settings set -secure "crypto.poloniex_secret_key" "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"

1. Setup min and max amount for accepted cash for one payment (defaults: min amount 1, max amount 15000) in currently accepted currency:
    1. Min limit:
    
			settings set "config.payments.limit_min" "<MIN AMOUNT OF THE ACCEPTED CASH>"
		
			Example - set 200 as min accepted cash in currently accepted currency: 
			settings set "config.payments.limit_min" "200"
    1. Max limit:

			settings set "config.payments.limit_max" "<MAX AMOUNT OF THE ACCEPTED CASH>"
			
			Example - set 15000 as max accepted cash in currently accepted currency: 
			settings set "config.payments.limit_max" "15000"			
    1. Max limit of accepted cash on the main page in currently accepted currency (default: 15000). You need to setup this limit separately for each fiat currency, accepted by the cash identification module:

			settings set "config.interface.menu.limit_max-><ISO-4217 SYMBOL CODE OF THE ACCEPTED FIAT CURRENCY>" "<MAX LIMIT FOR CASH ACCEPTANCE>"
		
			Example - setup max limit 14999 for "EUR" fiat currency: 
			settings set "config.interface.menu.limit_max->EUR" "14999"
1. Fiat currency setup for displaying prices/courses for cryptocurrencies on the main screen of the Trovemat software:

		settings set "config.payments.currency" "<ISO-4217 SYMBOL CODE OF THE ACCEPTED FIAT CURRENCY>"
	
		Example: Display all prices/courseson the main screen using US Dollars (USD):
		settings set "config.payments.currency" "USD"
1. Setup list of accepted fiat currencies and/or banknotes (default: all banknotes, accepted by cash identification module, is enabled):

		settings set "config.peripherals.validator->enabled_currencies" "<LIST OF ACCEPTED CURRENCIES/BANKNOTES>"
	
		Example: Enable to acceptance EUR banknotes of the following nominals: 20, 50, 100, 200:
		settings set "config.peripherals.validator->enabled_currencies" "EUR:20,EUR:50,EUR:100,EUR:200"
1. Setup the value of the convinience fee:
    1. Setup the condition for applying fee value for payments with the minimum amount value in the current currency:
    		
			settings set "config.payments.fee.part->min" "<MIN PAYMENT AMOUNT, FOR WHICH THAT FEE VALUE WILL BE APPLIED>"
			
			Example: Setup non-zero amount limit for that convinience fee rule:
			settings set "config.payments.fee.part->min" "0"
    1. Setup up convinience fee value in percents from payment amount:
    		
			settings set "config.payments.fee.part->percent" "<INTEGER NUMBER - PERCENT FROM PAYMENT AMOUNT FOR FEE>"
		
			Example: Setup fee 7% from payment amount:
			settings set "config.payments.fee.part->percent" "7"
    1. Setup fix fee rate in current currency:
			
			settings set "config.payments.fee.part->fix" "<INTEGER NUMBER - FIX FEE RATE>"
		
			Example: Setup fix fee rate 10 in currently accepted fiat currency:
			settings set "config.payments.fee.part->fix" "50"
1. Setup Kiosk parameters (name, address, etc):
    1. Owner's name:
    
			settings set "config.point_info.dealer_name->value" "<NAME OF THE KIOSK OWNER>"
    1. Owner's address:
    
    		settings set "config.point_info.dealer_address->value" "<KIOSK OWNER ADDRESS>"
    1. Owner's phone number:
    
    		settings set "config.point_info.dealer_phone->value" "<KIOSK OWNER PHONE NUMBER>"
    1. Address of the Kiosk's location:
    
    		settings set "config.point_info.point_address->value" "<ADDRESS OF THE KIOSK'S LOCATION>"  
1. Change the phone prefix number, displayed by default on the "Enter phone number" screen (for KYC procedure):

		settings set "config.interface->default_phone_code" "<ISO 3166-1 COUNTRY PHONE PREFIX>" 
		
		Пример: Setup phone prefix by default for Ukraine +38:
		settings set "config.interface->default_phone_code" "UA"
1.  Restart Kiosk to apply all previously entered settings: 
		
		service restart
