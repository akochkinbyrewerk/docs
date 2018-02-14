# Trovemat Kiosk User Guide

## Kiosk configuration files (configs)
Kiosk configuration files are located in the directory "configs".
* config.xml - Trovemat software configuration file
* menu.xml - Menu structure
* operators.xml - List of, so called, operators (concrete scenarios how to interact with the Kiosk client for selling cryptocurrency)
* crypto.xml - secure storage for settings
* *_lastgood.xml - last succesfully loaded file
* *_lastbad.xml - last unsuccesfully loaded file
	
Example:
1. If config.xml - correct. After successful validation Trovemat software will copy that file and save it as config_lastgood.xml.
1. If [operators.xml](#operators-list-operatorsxml) is wrong. After unsuccessful validation Trovemat software will copy that file and save it as operators_lastbad.xml. Then Trovemat software tries to load operators_lastgood.xml, and if it's valid - copy operators_lastgood.xml and save it as [operators.xml](#operators-list-operatorsxml)

## Trovemat software configuration (config.xml)
	
Example:
``` XML
<config>
    <parameters>
		<point_id>1</point_id>
		<extended_logging>false</extended_logging>
		<point_name>Trovemat kiosk #1</point_name>
	</parameters>
	<gateways>
		<jetcrypto_wallet type="trovemat" url="beta.jetcrypto.com" username="demo" password="crypto.jetcrypto_wallet_password" check="true" pay="true" tasks="3" tasks_interval="300" />
		<tox_messenger type="tox_text" >
			<friends>
				<users />
			</friends>
			<nodes>				
				<node_1 ip="144.76.60.215" port="33445" tox_id="04119E835DF3E78BACF0F84235B300546AF8B936F035185E2A8E9E0A67C8924F" />
				<node_3 ip="128.199.199.197" port="33445" tox_id="B05C8869DBB4EDDD308F43C1A974A20A725A36EACCA123862FDE9945BF9D3E09" />
				<node_4 ip="23.226.230.47" port="33445" tox_id="A09162D68618E742FFBCA1C2C70385E6679604B2D80EA6E84AD0996A1AC8A074" />
				<node_5 ip="2001:bc8:4400:2100::1c:50f" port="33445" tox_id="2C289F9F37C20D09DA83565588BF496FAB3764853FA38141817A72E3F18ACA0B" />
				<node_6 ip="178.21.112.187" port="33445" tox_id="4B2C19E924972CB9B57732FB172F8A8604DE13EEDA2A6234E348983344B23057" />
				<node_7 ip="163.172.136.118" port="33445" tox_id="2C289F9F37C20D09DA83565588BF496FAB3764853FA38141817A72E3F18ACA0B" />
			</nodes>
		</tox_messenger>	
	</gateways>
    <payments>
    	<gateway>jetcrypto_wallet</gateway>
		<currency>USD</currency>
		<limit_min>10</limit_min>
		<limit_max>3000</limit_max>
		<common_params>
			<poloniex_public_key>00000000000000000000000000000000000000000000000000000000</poloniex_public_key>
		</common_params>
    </payments>
	<interface wait_init_devices="2" lang="EN" >
		<menu vending="true" >
			<limit_max USD="3000" />
		</menu>
		<inactivity_timer data_entry="120" message="120" money_entry="120" />
	</interface>
    <terminal>
        <init>
            <app>./startup.sh</app>
            <timeout></timeout>
        </init>
    </terminal>
    <peripherals>
        <barcodereader model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" />
        <printer model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" charset_code_table="0" />
        <validator model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" />
    </peripherals>
    <point_info>
		<dealer_name name="" value="JetCrypto" />
		<dealer_address name="" value="Dzērbenes iela 14, Vidzemes priekšpilsēta, Rīga, LV-1006" />
		<dealer_phone name="" value="+371 66 555 098" />
		<point_address name="" value="Dzērbenes iela 14, Vidzemes priekšpilsēta, Rīga, LV-1006" />
    </point_info>
</config>
```

> Default values.
If parameter is missing in the configs, or it has value "default" - Trovemat software will load it's default value. 

> References to another parameter's value.
If parameter value begins with "config." - Trovemat software reads this value as reference to another parameter value. In the above example attribute "extended_logging" of tag "barcodereader" is a reference to the value of a parameter "extended_logging" in tag "parameters".

* parameters - common parameters
    * extended_logging - the most complete log files:
   	 > **!!! ATTENTION ! When you turn on extended logging, you must be aware of the fact, that all data will be logged in the text files, including sensetive data from devices, not intended to be stored in a usual text files !!!** 
		- **"false"** (default value) - turned off.
		- **"true"** - turned on.
    * point_id - Identification number of this Kiosk installation. Default value - 0.
    * point_name - Symbolic name of this Kiosk installation for displaying in messenger as a contact name. If this attribute not present or empty - name of the contact would be "Trovemat kiosk #<VALUE FROM ATTRIBUTE point_id>".
* payments - Default parameters for payments. Can be redefined for each operator in "money_entry" step inside operators.xml file. Parameters full description can be found at "[Operators](#operators-list-operatorsxml)" section.
    * gateway - Default value - "" - Gateway name.
    * currency - Default value - "USD".
    * limit_min - Default value - "0".
    * limit_max - Default value - "1000000".
    * payment_type - Default value - "mut_price".
    * fee - Missing by default.
    * check_name - Default value - "default.chq".
    * failed_check_name - Default value - "default_failed.chq".
    * common_params - List of parameters, that can be used in operators.xml file like "fields".
* interface
    * wait_init_devices - how to switch between initialization page and main page.
		- **"0"** (default value) - do not wait for devices to be initialized.
		- **"1"** - wait for initialization of the devices, that has parameter "show_errors" with value not equal "0".
		- **"2"** - wait for initialization of all devices.
	* lang - main page language by default
	* default_phone_code - ISO 3166-1 country code for "enter phone number" page. If the value of this attribute is empty - then country code field will not be filled up with the value. If this attribute is missing - then country code by default will be "+1"
	* menu - tag, describing main page behavior:
		* vending - default value "true" - flag, that allow cash acceptance on the main page without choosing any cryptocurrency (vending machine logic, when you first insert cash, than choose the snack that you want to buy)
		* limit_max - max cash amount accepted on the main page for each currency, attribute name is currency code, attribute value - is the max limit value for that currency.
	* inactivity_timer - Inactivity timeout in seconds for different pages:
		* data_entry - "Data entry" page - where client enters (or scan) it's wallet address
		* message - "Message" screen to client
		* money_entry - Cash acceptance page
* terminal
    * init - tag, described parameters for launching application when kiosk starts
        * app - string - application, that would be launched as a separate process when Trovemat software starts. You can write an arguments for that application separeted by spaces, in that string.
        * timeout - integer number  - timeout in msek for waiting application to start and finish. If value is "-1" - than that timeout is ignored.
* peripherals - describes models and parameters for the devices (e.g. cash identification module or receipt printer)
    * model - device model. Empty value means that there is no device of such type. Default value - "".
    * port - name of the serial port, used by this device. Default value - "".
    * baudrate - Baud rate of the serial port, used by this device. Default value - "9600".
    * extended_logging - the most complete log files, including all bytes transferred to and from device:
   	 > **!!! ATTENTION ! When you turn on extended logging, you must be aware of the fact, that all data will be logged in the text files, including sensetive data from devices, not intended to be stored in a usual text files !!!** 
		- **"false"** (default value) - turned off.
		- **"true"** - turned on.
    * show_errors - flag - show "Out of order" page when device have critical error(s):
		- **"0"** (default value) - do not show
		- **"1"** - show "Out of order" page only if there is no active payments or no unspent cash
		- **"2"** - show "Out of order" page immediately
* validator - Supported models (stored in "model" attribute):
    - **"mei_ebds"** - MEI cash identification module (aka note acceptor). Trovemat software supports firmware update for that model. When Trovemat software starts, it check for existence of file 'validator.bin' (firmware archive) in the root directory of Trovemat software installation, and, if that file exists, performs firmware update for the device. If firmware update was successfull, then that file is deleted. Supported attributes:
		- **enabled_currencies** - list of accepted currencies (if device firmware supports acceptance more than one currency). Empty value means that all supported by the device currencies can be accepted. Default value - empty string. Supported syntax:
			1. String with the delimeter "," (comma), it contains 3-symbols ISO-4217 currency codes. Example: "EUR,USD" - Allow acceptance of "EURO" (EUR, 978), "US Dollar" (USD, 840).
			1. String with the delimeter "," (comma), it contains banknotes descriptions, that allowed to accept by device. Syntax for banknote description "CUR:NOM", CUR - 3-symbols ISO-4217 currency code, NOM - nominal of that banknote in main unit of the currency. Example: "EUR:50,EUR:100,USD:10,USD:100" - allowed to accept "50 Euro" (EUR, 978, 50EUR), "100 Euro" (EUR, 978, 100EUR), "10 US Dollars" (USD, 840, 10USD), "100 US Dollars" (USD, 840, 100USD).
    - **"test_validator"** - test device emulated by Trovemat software for debug purpose.
* printer - Supported models (stored in "model" attribute):
	- **"tg2480"** - receipt printer CUSTOM TG2480. In some cases (depends on the printer's firmware) with the same settings receipt printer Custom VKP 80 II will be functional.
	- **"np-f3092d"** - Receipt printer Nippon NP-F3092D.
	- **"test_printer"** - test device emulated by Trovemat software for debug purpose.
    * charset_code_table - number of characters table, used for printing. Default value - "0".
        * For "tg2480" model:
            - **"0"** - CP437 (US)
            - **"2"** - CP850 (Multilingual)
            - **"3"** - CP860 (Portuguesse)
            - **"4"** - CP863 (Canadian-French)
            - **"5"** - CP865 (Nordic)
            - **"17"** - CP866 (Cyrillic)
            - **"19"** - CP858 (for Euro symbol 213)
* dispenser - Supported models (stored in "model" attribute):
    - **"puloon_lcdm"** - Banknotes dispenser "Poloon LCDM1000/2000" (1 and 2 cassettes respectively).
    - **"test_dispenser"** - test device emulated by Trovemat software for debug purpose (2 cassettes).
    * Following attributes must be specified for each cassette in dispenser (cassette number starts from 0):
    	* cassette_N - nominal of N-nth cassette, for example cassette_0="100 USD".
    	* default_capacity_N - banknotes count by default for N-nth cassette (admin can change that value during service procedures), for example: default_capacity_0="1000".
		
* point_info - In that section you can setup unlimited parameters with any name. These parameters will be interpreted as fields during client session. These fields can be used in receipts and in requests to gateways. "Id" of the field has name like _INFO_*, where * - name of the field written in uppercase letters. Name and value of the field can be setup by "name" and "value" attributes. Example: <dealer_name name="Dealer" value="Trovemat services seller, Gmbh" /> will be translated into field with id="_INFO_DEALER_NAME", name="Dealer" и value="Trovemat services seller, Gmbh".

* gateways - In that section you can setup unlimited number of tags, describing connections to different servers. Name of child tag - it's the name of the gateway, which can be used in steps descriptions (see attribute "gateway" of tag "step"). For each gateway you have to specify following attributes:
	- type - Gateway type. Available types: 
		- **"trovemat"** - gateway for accessing api.jetcrypto.com.
			* Additional attributes for that gateway:
				- url - server address		
				- username - login for JetCrypto account
				- password - password for JetCrypto account
				- tasks_interval - New tasks request period (seconds). Default value - "1". 
		- **"tox_text"** - gateway for accessing TOX network for Trovemat software.
			* Additional attributes for that gateway:
				- udp_enabled - use UP protocol. Default value - "true".
				- local_discovery_enabled - search for another tox-nodes inside local networl. Default value - "true".
				- hole_punching_enabled - use "hole-punching" method for search of another tox-nodes. Default value - "true".
				> **Attributes "udp_enabled", "local_discovery_enabled", "hole_punching_enabled" can be disabled in order to minimize network traffic, but in that case you need to use tcp connection to node, that supports tcp relay.**
				- users - list of trusted tox-accounts. Name of child tag - name of trusted account.
					- tox_id - id of tox contact
					- message_queue_limit - size of the unsent message queue for that tox contact (if Trovemat software or tox account is offline). Default value - "100".
				- nodes - node list for connecting to tox network. Name of the child tag - is the name of the node.
					- ip - ip address of the node
					- port - port number of the node
					- tox_id - public key of the node
					- tcp_relay - flag - connecting to tox node using TCP. Will work only if node can accept TCP connections and for that gateway UDP is disabled (udp_enabled is "false"). Default value - "false".
		- Some attributes for gateways tags can be set for gateways without users ("trovemat" gateway, for example) and in users tags for gateways that works with users ("tox_text" for example):
			- check - flag - enables gateway without users for info queries from Trovemat software. Default value - "false".
			- pay - flag - enables gateway without users for payment queries from Trovemat software. Default value - "false".
			- tasks - attribute for user or gateway tag - defines permission level for current user/gateway:
				- **"-1"** (default value) - all forbidden.
				- **"0"** - info requests, not changing Kiosk state or settings
				- **"1"** - service commands (restart, shut down, etc).
				- **"2"** - change Trovemat software settings
				- **"3"** - running scripts in OS using Trovemat software account credentials.
			- info_task - applied to user - filters all notification below that level about executing tasks by other users. For example, if this attribute set to "2", than that user will receive notification, when other user tries to execute task with levels "2", "3" and will not receive notifications about tasks with levels "0" and "1". Default value - "2".
			- info_factor - flag - applied to user - receive notifications about add/remove new state factors. Default value - "false".
			- info_full_state - flag - applied to user - receive notifications about changing of a detailed state (added or removed one of the factors) of the Trovemat software (for example, if receipt printer is low on paper, this will be treated as change of a detailed state) Default value - "false".
			- info_state - flag - applied to user - receive notifications about changing of a overall state of the Trovemat software (e.g. it's in the error state, or everything fine) - this state also used as a state of a TOX account for that Trovemat instance. Default value - "false".
			- info_bill - flag - applied to user - receive notifications about accepting/stacking banknotes into cassette. Default value - "false".
			- info_payment - flag - applied to user - receive notifications about new payments. Default value - "false".
    
## Menu (menu.xml)
Menu contains groups and operators. Allowed only 'group' or 'operator' tag inside that file. Attribute 'type' is required for all tags. All operators in menu must be described in operators list [operators.xml](#operators-list-operatorsxml). Operator description in menu and in operators list [operators.xml](#operators-list-operatorsxml) must have same tag name.

Attributes:
* "type" - type of an item. Possible values: "group" - group of operators - when user press that button in the menu, list of items from that group will be displayed to client, "operator" - concrete operator - when user press that button in the menu, Trovemat software will begin to execute scenario for that operator (scenario described in operators list [operators.xml](#operators-list-operatorsxml)).
* "name" - Group name. Value must contain key name from file with localized string interface/js/language.js for current locale. This name displayed on the screen with group items.

Example of menu.xml for menu, which contains 8 operators on the main page:
``` XML
<?xml version="1.0" encoding="utf-8"?>
<menu type="group" name="choose_your_currency" >
	<bitcoin type="operator" />
	<ethereum type="operator" />
	<dash type="operator" />
	<ripple type="operator" />
	<monero type="operator" />
	<ethereum_classic type="operator" />
	<litecoin type="operator" />
	<nem type="operator" />
</menu>
```

## Operators list (operators.xml)

* operator - Operator. Describes payments scenario for that operator. Name of this tag can be in any format, according to XML rules.
	* step - Step of the scenario.
		* type - attribute - Type of the step. Required attribute. No defauде value. Possible values:
			- **"data_entry"** - User enters some payment details on that step (e.g. wallet address).
			- **"choose_currency"** - Step for chosing fiat currency to insert by the client. User can choose any fiat currency (supported by Trovemat software) from the list. 
                * mode - Currency list, user can choose from:
					1. **"USD,RUB,EUR"** - List of currencies to display to client.
					1. **"NOTE_VAL"** (default value) - Display all currencies, accepted by cash identification module.			
			- **"money_entry"** - Insert fiat cash by the client. Also you can redefine payment parameters default values, saved in conig.xml -> payments:
				* payment_type - Type of payment. Possible values:
					- **"mut_price"** - not fixed price.
					- **"fix_price"** - fixed price.
				* price - item price (for fixed price operator). No default value. Required in case of **type="fix_price"**.
				* currency - 3-symbols ISO-4217 currency code.
                * limit_min - Min accepted amount in fiat currency.
				* limit_max - Max accepted amount in fiat currency.
                * network_fee - fixed network fee in cryptocurrency, added to convinience fee.
				* fee - Convinience fee, can be setup with different values depends on the amount of inserteв fiat currency.
					* part - Interval for concrete fee value:
						* min - min amount of inserted cash. When reached, this fee rate begins to be active for that payment.
						* percent - Percent value for fee.
						* fix - Fix fee rate.
						
					Example:
					From 0 to 50 - Convinience fee 2% + 2.01 fix
					From 50 and more - Convinience fee 1%
					``` XML
					<fee>
						<part_1 min="0" percent="2" fix="2.01" />
						<part_2 min="50" percent="1" fix="0" />
					</fee>
					```
			- **"check"** - Request for conversion rate from cryptocurrency to fiat from exchange.
				* server - Server name, where to send this request. Values for accessing server saved in conig.xml -> network -> <SERVER NAME>
				* path - Request URL (path). E.g.: "/api/Method"
				* method - Request method. Possible values:
					- **"POST"** - POST request. Request body will contain JSON-object, containing fields, described in tags with attribute "type" equals to "request_field" value inside current step.
					- **"GET"** - GET request. Request body will be empty. Request URL will be filled up with fields, tags with attribute "type" equals to "request_field" value inside current step.
			- **"pay"** - Request to gateway for payment (withdrawal operation from Trovemat owner's wallet to client wallet). Parameters for this step are the same, as in "check" step.
			- **"print"** - Print receipt.
				* check_name - name of the receipt template, default value stored in config.xml.
			- **"message"** - Show some text message to client.
		- All child tags with attribute "type" equals to "request_field" - used for steps with type "check" and "pay":
			- id - field id. If value contains symbol "." (point) - in the request with method="POST" this field will be saved as nested elements of JSON object. For example, field with id "parameters.token" will be sent as JSON object inside requests body: {"parameters" : {"token" : "123123123123"}}
			- value - Value for request field. Inside that value you can place reference to another field or data, received on any previous step (from user or from gateway). Reference to field, filled by client, can be described as "|<NAME OF THE TAG WITH FIELD, FILLED BY USER>|". Reference to field, received from gateway (tag with attribute "type" equals to "receive_field") can be described as "|<TAG NAME WITH ATTRIBUTE 'TYPE' EQUALS TO 'receive_field'>|". Example: Assume that scenario executes step with type **"data_entry"** and on that step user enters value for tag with the name 'field_wallet'. Than in any followed steps we can get that value by using "|field_wallet|" substring. For example, string "01-|field_wallet|" - will be translated to "01-123" in case when user inputs "123" as wallet address on data entry step with field "field_wallet".
		- All child tags with attribute "type" equals to "receive_field" - field, that Trovemat software receives from gateway in answer to request.
			- tag name - name of the parameter in server response. When Trovemat software receives response from server, it creates in current client session parameter with name, equal to tag name. This parameter will exists only during current client session. This name can be referenced in any other field. Example: gateway response contains field with the name "buy". In step description we use tag <buy type="receive_field" name="BUY_RECEIVED" />". Then in any other following step, e.g. in step with type "request_field", we can reference to that parameter as follows: "|BUY_RECEIVED|" - Trovemat software replace this string with the value of the received parameter.
			- name - name of the parameter inside this step. 

Example of operators.xml, contains 2 operators:
``` XML
<?xml version="1.0" encoding="utf-8"?>  
<operators>
	<bitcoin name="bitcoin" long="bitcoin" short="BTC" image="btc.png" >
		<step_0 type="check_printer" />
		<step_1 type="money_entry" />
		<step_2 type="data_entry" >
			<field_wallet name="Wallet" barcode_title="scan_your_wallet" title="enter_wallet_address" input_type="barcode" keyboard="text" regexp="^[13][a-km-zA-HJ-NP-Z1-9]{25,34}$" validChars="[a-km-zA-HJ-NP-Z1-9]" control_buttons="SHIFT DEL" language="EN" />
		</step_2>
		<step_3 type="data_entry" >
			<field_phone name="Phone number" title="enter_your_phone_number" keyboard="numbers" regexp="(9[976]\d|8[987530]\d|6[987]\d|5[90]\d|42\d|3[875]\d|2[98654321]\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\d{1,14}$" />
		</step_3>
		<step_4 type="check" path="/api/trovemat/Phone/verify" method="GET" >
			<phoneNumber type="request_field" value="|field_phone|" />
			<message type="request_field" value="Trovemat verification code: {0}" />
			<verifyCode type="receive_field" name="VERIFYCODE_RECEIVED" />
		</step_4>
		<step_5 type="data_entry" >
			<field_code name="Verification code" title="enter_verification_code" keyboard="numbers" print="0" regexp="[0-9]{4,}" />
		</step_5>
		<step_6 type="check" path="validate_verification_code" >
			<VERIFYCODE_SRC type="request_field" value="|verifyCode|" />
			<VERIFYCODE_DST type="request_field" value="|field_code|" />
		</step_6>
		<step_7 type="pay" path="/api/trovemat/Payment" method="POST">
			<operatorId type="request_field" value="31" />
			<params type="request_field" secure="true" value="{&quot;currency&quot;:&quot;BTC&quot;,&quot;address&quot;:&quot;|field_wallet|&quot;,&quot;secretKey&quot;:&quot;|_CRYPTO_poloniex_secret_key|&quot;,&quot;publicKey&quot;:&quot;|_COMMON_poloniex_public_key|&quot;,&quot;withdrawalAmount&quot;:&quot;|WITHDRAWAL_AMOUNT|&quot;,&quot;phoneNumber&quot;:&quot;|field_phone|&quot;}" />
			<currencyId type="request_field" value="|_CURRENCY_CODE|" />
			<amount type="request_field" value="|_ACCEPTED_MINOR_UNIT|" />
		</step_7>
		<step_8 type="print" check_name="poloniex.chq" failed_check_name="poloniex_failed.chq" />
		<step_9 type="message" />
	</bitcoin>
	<ethereum name="ethereum" long="ethereum" short="ETH" image="ethereum.png" >
		<step_0 type="check_printer" />
		<step_1 type="money_entry" />
		<step_2 type="data_entry" >
			<field_wallet name="Wallet" barcode_title="scan_your_wallet" title="enter_wallet_address" input_type="barcode" keyboard="text"  regexp="^(0x)?([0-9a-f]{40})|([0-9A-F]{40})$" validChars="[a-fA-FxX0-9]" control_buttons="SHIFT DEL" language="EN" />
		</step_2>
		<step_3 type="data_entry" >
		<field_phone name="Phone number" title="enter_your_phone_number" keyboard="numbers" regexp="(9[976]\d|8[987530]\d|6[987]\d|5[90]\d|42\d|3[875]\d|2[98654321]\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\d{1,14}$" />
		</step_3>
		<step_4 type="check" path="/api/trovemat/Phone/verify" method="GET" >
			<phoneNumber type="request_field" value="|field_phone|" />
			<message type="request_field" value="Trovemat verification code: {0}" />
			<verifyCode type="receive_field" name="VERIFYCODE_RECEIVED" />
		</step_4>
		<step_5 type="data_entry" >
			<field_code name="Verification code" title="enter_verification_code" keyboard="numbers" print="0" regexp="[0-9]{4,}" />
		</step_5>
		<step_6 type="check" path="validate_verification_code" >
			<VERIFYCODE_SRC type="request_field" value="|verifyCode|" />
			<VERIFYCODE_DST type="request_field" value="|field_code|" />
		</step_6>
		<step_7 type="pay" path="/api/trovemat/Payment" method="POST">
			<operatorId type="request_field" value="31" />
			<params type="request_field" secure="true" value="{&quot;currency&quot;:&quot;ETH&quot;,&quot;address&quot;:&quot;|field_wallet|&quot;,&quot;secretKey&quot;:&quot;|_CRYPTO_poloniex_secret_key|&quot;,&quot;publicKey&quot;:&quot;|_COMMON_poloniex_public_key|&quot;,&quot;withdrawalAmount&quot;:&quot;|WITHDRAWAL_AMOUNT|&quot;,&quot;phoneNumber&quot;:&quot;|field_phone|&quot;}" />
			<currencyId type="request_field" value="|_CURRENCY_CODE|" />
			<amount type="request_field" value="|_ACCEPTED_MINOR_UNIT|" />
		</step_7>
		<step_8 type="print" check_name="poloniex.chq" failed_check_name="poloniex_failed.chq" />
		<step_9 type="message" />
	</ethereum>
</operators>
```

## Receipt templates (*.chq)

In text of receipt templates it is possible to use variables (aliases to fields, fillied by operator scenario).

Variables syntax rules:
1. Variable name must be enclosed in percent sign ("%").
1. Name of the variable may consist of: latin uppercase letters, numbers, underscore sign ("_").
1. After variable name it is possible to set modifiers using following rules:
	1. Modifiers separeted by colon sign (":")
	1. Modifiers must be mentioned in strict order, described below.
        1. Uppercase latin letter "N" or "V". "N" means that variable would be substituted by field name. "V" means that variable would be substituted by field value. Default value: "V".
        1. Number. Max length of replaced string. If this number is bigger than value length, than the string would be completed by spaces until specified length. If this number is less than value length, than the string would be truncated to specified length.
        1. Uppercase latin letter "R" or "L". "R" (right) or "L" (left) defines the side, where the string would be truncated or  completed up to specified length. Default value: "L".

Example:
Receipt template:
```
--------------------------------
%SUM%
%SUM:N%: %SUM:V%
Value of the field SUM: %SUM:V%
%SUM:N:2:R%.: %SUM:V:5%
%SUM:20%
--------------------------------
```
Let's assume that we have define the following field:  
<sum name="Amount" value="1234567890" />
Printed receipt will look like the following text:
```
--------------------------------
1234567890
Amount: 1234567890
Value of the field SUM: 1234567890
Am.: 67890
          1234567890
--------------------------------
```
User-defined variables.
1. Can't start with the underscore "_" sign.
1. All defined fields is treated as user variables. Name of the tag is the name of the variable.

System-defined variables.
All variables, begins with uderscore sign "_" is system. Avaialable variables:
* _FIELDS_FOR_PRINT - Prints all user-defined variables, which has attribute "print" with value "1", in the following form: "variable_name: variable_value".
* _INFO_* - fields, from configuration file config.xml -> config -> point_info.
* _TERMNUMBER - Kiosk number (config.xml -> config -> parameters -> point_id)
* _DATETIME - Current date and time
* _ACCEPTED - Total amount of accepted cash, starting from last opened incassation period.
* _FEE - Fee
* _CREDIT - Total amount to be converted to cryptocurrency (_ACCEPTED minus _FEE)
* _CURRENCY - 3-symbols ISO-4217 currency code of the inserted cash
* _OPNAME - name of the operator

Only for collection receipt template:
* _INCS_LAST_INCASSATION_TIME - предыдущее время инкассации
* _INCS_INCASSATION_TIME - время текущей инкассации
* _INCS_MONEY_INFO - detailed information (with counters) about accepted banknotes.
* _INCS_GOODS_INFO - detailed information (with counters) about sold cryptocurrencies

## Logs (Logs/*.log)

Stored in "logs" directory, located at the application level. Name of each log file includes name of the log object and date. Each log line consist of message type, timestamp, thread id, number of line in code and message text.

Objects that logs events:
* Kiosk - Main application log file.
* Main - Critical information about Trovemat software.
* Device_* - Devices log (name of the device is a part of a log file name).
	
Message types:
* INF - information level.
* WRN - warning.
* ERR - error.
* EXT - Extended message, appears only if extended logging enabled.
	
## Device emulation

For demo version of a Trovemat software it is possible to use software-emulated devices, such as receipts printer and cash identification module. For each device type there are special model name for such virtual devices. To emulate device events you have to use functional keys on hardware keyboard (F1-F12). First key press choses device, second - emulates device event. Interval between key presses must not exceed 5 seconds. Virtual device events can be emulated only in demo version of Trovemat software. Virtual devices must be used only with test purposes!

* Validator.
    * model - "test_validator"
    * key - "F9"
    * events:
        * F1 - device is offline
        * F2 - device online
        * F3 - cassette removed
        * F7 - Inserted 1 minimum unit for chosen currency
        * F8 - Inserted 10 minimum unit for chosen currency
        * F9 - Inserted 100 minimum unit for chosen currency
        * F10 - Inserted 500 minimum unit for chosen currency
        * F11 - Inserted 1000 minimum unit for chosen currency
        * F12 - Inserted 5000 minimum unit for chosen currency
		
* Printer - Prints receipts into text files (temp/receipt_*.txt)
    * model - "test_printer"
    * key - "F11"
    * events:
        * F1 - device is offline
        * F2 - device online
	
* Dispenser.
    * model - "test_dispenser"
    * key - "F12"
    * events:
        * F1 - device is offline
        * F2 - device online
        * F3 - cassette removed
        * F4 - dispense number of banknotes, equal to the nominal of the banknotes from of the 0-nth cassette	

## Service keys
* F1 + F1 - Close Trovemat software for access to system console
* F1 + F2 - Enter Trovemat software service menu
* F1 + F3 - Restart Trovemat software without restarting OS
* F1 + F4 - Launch touch-screen calibrate procedure
* F9 + ... - Virtual cash identification module command - see [Device emulation](#device-emulation)
* F10 + ... - Virtual QR-code scanner - see [Device emulation](#device-emulation)
* F11 + ... - Virtual receipt printer - see [Device emulation](#device-emulation)
* F12 + ... - Virtual banknotes dispenser - see [Device emulation](#device-emulation)

## Demo version limitations
1. SMS sends through server [jetcrypto.com](https://jetcrypto.com/) using "Trovemat" sender name.
1. There are big red flashing message on the top of the application about demo version.
1. Application automatically shuts down after 10 minutes of work (can be changed upon request on sales@trovemat.com).

## Особенности работы приложения
1. Phone number must be entered according to mask in input field (withut country code). Country code must be chosen from a list of countries.
1. Exchange rates on the main page are loaded from cryptocompare.com
1. Access to cryptocurrency exchanges performed with the access tokens (keys), specified for each exchange separately. Parameters name for storing access keys see at [Settings for crytpocurrency exchange](#settings-for-crytpocurrency-exchange).

## Калибровка сенсорного экрана (touch screen)

Процедура калибровки сенсорного экрана может понадобиться в случае если сенсорный экран некорректно фиксирует координаты нажатия. Это может проявляться в том, что нажатия на сенсорный экран затруднены (сложно "попасть" в кнопки управления интерфейса ПО Trovemat), а также может проявляться в некорреткной работе интерфейса ПО Trovemat. 
Если после проведения калибровки сенсорного экрана проблема не устраняется, то, возможно, края сенсорного экрана загрязнены и требуют очистки специалистом по обслуживанию платежных киосков или банкоматов, либо сенсорный экран вышел из строя.

### Утилита калибровки

Запустить утилиту калибровки сенсорного экрана можно посредством нажатия последовательно клавиш F1 и F4 на физической клавиатуре во время работы ПО Trovemat на главной стартовой странице.

### Ручной запуск калибровки сенсорного экрана из командной строки

Откалибровать сенсорный дисплей: 
1. Выполнить команду 
    ```Shell
    sudo apt install xinput-calibrator && xinput_calibrator --list
    ```
    и посмотреть id Тачскрина.
1. Выполнить команду 
    ```Shell 
    xinput_calibrator --device <ИДЕНТИФИКАТОР УСТРОЙСТВА, ОПРЕДЕЛЁННЫЙ НА ПЕРВОМ ШАГЕ>
    ```
1. Чтобы сохранить калибровку, нужно скопировать полученную после калибровки информацию (начиная с Section "InputClass" и заканчивая EndSection, включая эти фразы)
1. Выполнить команду 
    ```Shell 
    sudo nano /usr/share/X11/xorg.conf.d/99-calibration.conf
    ```
    Возможно, необходимо будет предварительно нажать "Y", чтобы подтвердить действие, соответствующая подсказка будет на экране терминала.
1. Вставить скопированную информацию, сохранить запись (CTRL+"O" (буква O англ.)) и подтвердить клавишей ENTER, далее выход (CTRL + X)

## Настройка списка администраторов для управления киоском

Для управления ПО Trovemat может использоваться любй мессенджер, поддерживающий протокол [Tox](https://tox.chat/). Управление списком пользователей, которые имеют возможность взаимодействовать с киоском, осуществляется следующим образом:
1. Перейти в сервисный режим (см. [Service keys](#service-keys))
1. Нажать кнопку "Список администраторов"
1. Открыть QR-код, содержащий tox-id пользователя, которого надо добавить в список администраторов киоска
1. Сканировать открытый QR-код на киоске
1. Подтвердить в мессенджере администратора запрос на добавления в список контактов от ПО Trovemat

Управление ПО Trovemat с использованием мессенджера:
1. Можно отправить команду
	```
	help
	```
	в ответ киоск пришлёт список команд, которые он может выполнять
1. Для получения подробного описания по отдельной команде, необходимо отправить сообщение вида
	```
	help 'НАИМЕНОВАНИЕ КОМАНДЫ'
	```
	Например, команда:
	```
	help 'status'
	```
	В результате выполнения этой команды ПО Trovemat пришлёт расширенное описание команды "status"

## Settings for crytpocurrency exchange

### [Poloniex](https://poloniex.com)

API Key stored in parameter "config.payments.common_params.poloniex_public_key"

Secret stored in parameter "crypto.poloniex_secret_key"

### [Bitfinex](https://www.bitfinex.com/)

API key stored in parameter "config.payments.common_params.bitfinex_public_key"

API key secret stored in parameter "crypto.bitfinex_secret_key"

### [Bittrex](https://bittrex.com)

API key stored in parameter "config.payments.common_params.bittrex_public_key"

API key secret stored in parameter "crypto.bittrex_secret_key"

### [EXMO](https://exmo.com)

Public key stored in parameter "config.payments.common_params.exmo_public_key"

Secret key stored in parameter "crypto.exmo_secret_key"

## Exit to command line from Trovemat software

1. Close Trovemat software (see. [Service keys](#service-keys))
1. Press ALT + F4
1. Press "Logout" button
1. When you're done with the command line - run command "exit" to launch Trovemat software or just simply reboot
