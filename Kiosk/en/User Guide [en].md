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
1. If [operators.xml](#Operators-list-operatorsxml) is wrong. After unsuccessful validation Trovemat software will copy that file and save it as operators_lastbad.xml. Then Trovemat software tries to load operators_lastgood.xml, and if it's valid - copy operators_lastgood.xml and save it as [operators.xml](#Operators-list-operatorsxml)

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
* payments - Default parameters for payments. Can be redefined for each operator in "money_entry" step inside operators.xml file. Parameters full description can be found at "Operators" section.
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
		
* point_info - В данном разделе можно задать неограниченное количество системных полей с любыми именами. Эти поля можно использовать при печати чеков и запросах к шлюзам. "Id" поля задается вида _INFO_*, где * - имя поля заглавными буквами. Имя и значение могут быть заданы атрибутами "name" и "value". Пример: <dealer_name name="Дилер" value="Рога и Копыта" /> будет преобразовано в системное поле с id="_INFO_DEALER_NAME", name="Дилер" и value="Рога и Копыта".

* gateways В данном разделе можно задать неограниченное количество тегов, описывающих подключение к различным серверам.
Название вложенного тега - это название шлюза, которое можно использовать в шагах (см. тег "step" атрибут "gateway"). Для каждого шлюза указыватся следующие аттрибуты:
	- type - наименование типа шлюза. Доступные типы: 
		- **"trovemat"** - шлюз для доступа к api.jetcrypto.com.
		Для данного шлюза указываются следующие дополнительные аттрибуты:
			- url - адрес сервера		
			- username - учётная запись для доступа
			- password - пароль для доступа к API
			- tasks_interval - интервал опроса шлюза на предмет новых задач
		- **"tox_text"** - шлюз для доступа к терминалу через любой tox-мессеннджер.
			* Для данного шлюза указываются следующие дополнительные атрибуты:
				- udp_enabled - использование UDP протокола. Значение по умолчанию - "true".
				- local_discovery_enabled - поиск других tox-узлов внутри локальной сети. Значение по умолчанию - "true".
				- hole_punching_enabled - использование метода "hole-punching" для поиска других tox-узлов. Значение по умолчанию - "true".
				- Все три атрибута (udp_enabled, local_discovery_enabled, hole_punching_enabled) - можно отключить для уменьшения трафика, однако в этом случае для выхода в сеть необходимо tcp подключение к узлу, поддерживающему tcp relay.
			* Для данного шлюза указываются следующие дополнительные теги:
				- users - список доверенных tox-аккаунтов. Наименование вложенного тега - имя такого аккаунта.
					- tox_id - идентификатор tox-аккаунта
					- message_queue_limit - размер очереди неотправленных сообщений (если терминал или пользователь находятся вне сети). Значение по умолчанию - "100".
				- nodes - список узлов для подключения к tox-сети. Наименование вложенного тега - имя такого узла.
					- ip - ip адрес узла
					- port - порт узла
					- tox_id - публичный ключ узла
					- tcp_relay - подключение к узлу по TCP. Имеет смысл только если узел поддерживает данную функцию и для шлюза отключен UDP. Значение по умолчанию - "false".
		- Ряд атрибутов указывается в корневом теге для шлюзов без пользователей (например "trovemat") и в тегах описывающих пользователей для шлюзов с пользователями (например "tox_text"):
			- check - разрешение на использование данного шлюза для информационных запросов с киоска. Значение по умолчанию - "false".
			- pay - разрешение на использование данного шлюза для платежных запросов с киоска. Значение по умолчанию - "false".
			- tasks - уровень команд, разрешенных к выполнению со стороны шлюза (указанный уровень и ниже).
				- **"-1"** (значение по умолчанию) - запрещены все.
				- **"0"** - просмотр информации, не влияющий на его работу.
				- **"1"** - сервисные команды (выключение, перезагрузка и т.п.).
				- **"2"** - изменение параметров терминала.
				- **"3"** - выполнение скриптов на терминале от имени администратора.
			- info_task - уровень команд, при выполнении которых другими пользователями, вы получите уведомления (указанный уровень и выше). Значение по умолчанию - "2".
			- info_factor - получение уведомлений о добавлении/удалении новых факторов состояния. Значение по умолчанию - "false".
			- info_full_state - получение уведомлений о изменении полного состояния терминала (состоящего из суммы всех текущих факторов состояния). Значение по умолчанию - "false".
			- info_state - получение уведомлений о изменении простого состояния терминала (Ok, Error, т.п.). Значение по умолчанию - "false".
			- info_bill - получение уведомлений о получении купюр купюроприемником. Значение по умолчанию - "false".
			- info_payment - получение уведомлений о платежах. Значение по умолчанию - "false".
    
## Меню (menu.xml)
Меню состоит из групп и операторов, соответственно в конфиге разрешены только теги типа 'group' или 'operator'. Обязательным для всех элементов является аттрибут 'type'. Все операторы, присутствующие в меню, должны быть и в списке операторов [operators.xml](#Список-операторов-operatorsxml). Оператор в файле [operators.xml](#Список-операторов-operatorsxml) определяется по наименованию его тега.

Аттрибуты:
* "type" - тип элемента. Возможные значения: "group" (группа операторов), "operator" (конкретный оператор).
* "name" - Наименование группы. Значением атрибута является ключ фразы из файла с локализованными сообщениями interface/js/language.js для текущей выбранной локали

Пример файла menu.xml для меню, которое состоит из 8-ми операторов на главной странице:
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

* operator - оператор. Описывает схему проведения платежа. Название данного тега может быть любым, не противоречащим правилам наименования XML-тегов.
	* step - шаг платежа.
		* type - тип шага платежа. Обязательный параметр. Значение по умолчанию отсутствует. Допустимые значения:			
			- **"data_entry"** - ввод данных платежа пользователем.
			- **"change_currency"** - выбор валюты. Пользователь сам выбирает валюту из списка. 
				* mode - Возможные значения (способы формирования списка валют):
					1. **"USD,RUB,EUR"** - Перечисление списка валют через запятую.
					1. **"NOTE_VAL"** (значение по умолчанию) - все валюты, поддерживаемые купюроприёмником.			
			- **"money_entry"** - внесение денег пользователем. Так же тут задаются параметры платежей, значения по умолчанию для которых заданы в conig.xml -> payments.
				* payment_type - тип платежа. Возможные значения:
					- **"mut_price"** - цена не фиксированна.
					- **"fix_price"** - цена фиксированна.
				* price - цена (для фиксированного платежа). Значение по умолчанию отсутствует. Обязательно к заполнению при **type="fix_price"**.
				* currency - валюта платежа, трехбуквенный код по ISO 4217.
				* limit_min - минимальная сумма для приёма наличных.
				* limit_max - максимальная сумма для приёма наличных.
				* fee - комиссия, задается промежутками по диапазону принятых денег.
					* part - один из диапазонов, для каждого из которых задается своя комиссия.
						* min - порог, с которого начинает взыматься данная комиссия.
						* percent - процентная составляющая коммиссии.
						* fix - фиксированная составляющая коммиссии.
						
					Пример:
					От 0 до 50 у.е. - взымать коммиссию в размере 2% + 2.01 у.е.
					От 50 и выше - взымать коммиссию в размере 1%.
					``` XML
					<fee>
						<part_1 min="0" percent="2" fix="2.01" />
						<part_2 min="50" percent="1" fix="0" />
					</fee>
					```
			- **"check"** - запрос к шлюзу без платежа.
				* server - Наименование сервера, на который необходимо отправлять запрос. Конкретные параметры для доступа к серверу находятся в conig.xml -> network -> <НАИМЕНОВАНИЕ СЕРВЕРА>
				* path - строка запроса (адрес). Например: "/api/Method"
				* method - метод запроса. Возможные значения:
					- **"POST"** - будет отправлен POST запрос. Тело запроса будет содержать JSON-объект, составленный из полей, описанных в тегах "request_field" данного шага.
					- **"GET"** - будет отправлен GET-запрос. Тело запроса будет пустым. URL запроса будет составленн из полей, описанных в тегах "request_field" данного шага.
			- **"pay"** - запрос к шлюзу и осуществление платежа. Параметры данного шага аналогичны параметрами шагов с типом "check"
			- **"print"** - печать чека.
				* check_name - имя файла шаблона чека, значение по умолчанию в config.xml.
			- **"message"** - сообщение пользователю.
		- request_field - тег - используется для шагов с типом "check" и "pay" как поле запроса:
			- id - идентификатор поля. Если содержит символ "." (точка) - то в запросе с типом method="POST" данный параметр будет сохраняться как вложенные элемента JSON-объекта. Например, если указан параметр parameters.token то будет отправлен следующий JSON объект в теле запроса: {"parameters" : {"token" : "123123123123"}}
			- value - содержит значение параметра запроса. Внутри значения можно записать "ссылку" на данные, полученные на другом шаге (от пользователя или от шлюза). Ссылка на поле, введённое клиентом, описывается как "|fi_\<Значение атрибута id тега field\>|". Ссылка на поле, полученное от шлюза, описывается как : "|\<НАИМЕНОВАНИЕ_ПАРАМЕТРА\>|". Например, если был выполнен шаг с типом **"data_entry"** и на этом шаге клиент ввёл значение для поля с id="2", то значение может выглядеть как "001-|fi_2|" - в этом случае если клиент ввёл значение "123" на сервер будет отправлена строка "001-123".
		- receive_field - поле, которое присылает шлюз в ответе на запрос.
			- id - имя параметра в ответе от сервера. Также в текущей клиентской сессии создаётся парамер с данным именем. Данное имя будет актуально только в течении текущей клиентской сессии. Данное имя может быть использовано в ссылке. Например, шлюз в ответе присылает поле с наименованием "buy". В шаге будет указано "...<receive_field id="buy" name="COURSE" />...". Далее в любом другом шаге, например в теге "request_field" можно использовать значение данного параметра путём указания строки "|buy|" - вместо этой строки будет подставлено конкретное значение, полученное от шлюза.
			- name - наименование параметра внутри данного сценария. 

Пример файла operators.xml, состоящего из двух операторов:
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

## Шаблоны чеков (*.chq)

В шаблонах чеков можно использовать переменные.

Правила записи переменных:
1. Имя переменной заключено в знаки процента "%".
1. В имени переменной можно использовать латинские заглавные буквы, цифры, знак подчеркивания.
1. После имени переменной могут идти параметры к переменной.
	Правила записи параметров к переменным:
	1. Перед каждым параметром должно стоять двоеточие ":".
	1. Параметры должны идти строго в порядке их перечисления ниже.

Доступные параметры:
1. Заглавная латинская буква "N" или "V". Определяет имя ("N") или значение ("V") поля необходимо вывести. Значение по умолчанию: "V".
1. Число, задает длину выводимого значения. Значение переменной будет обрезано или дополнено пробелами до указанной длины.
1. Заглавная латинская буква "R" или "L". Опеределяет справа ("R") или слева ("L") необходимо обрабатывать поле (убирать лишние символы или добавлять пробелы до нужной длины). Значение по умолчанию: "L".

Пример обработки и вывода переменных.  
Шаблон:
```
--------------------------------
%SUM%
%SUM:N%: %SUM:V%
Значение поля сумма: %SUM:V%
%SUM:N:3:R%.: %SUM:V:5%
%SUM:20%
--------------------------------
```
Пусть задано следующее поле:  
id="SUM" name="Сумма" value="1234567890"
Тогда результат будет следующим:
```
--------------------------------
1234567890
Сумма: 1234567890
Значение поля сумма: 1234567890
Сум.: 67890
          1234567890
--------------------------------
```
Пользовательские переменные.
1. Не могут начинаться со знака подчеркивания "_".
1. В пользовательские переменные добавляются все поля операторов (field). Идентификаторы задаются вида "fi_id" (fi_23 для field id="23").

Системные переменные.
Переменные, начинающиеся со знака подчеркивания "_" являются системными. Доступны следующие системные переменные:
* _FIELDS_FOR_PRINT - выводит все пользовательские переменные, у которых значение "print" равно "1", в виде набора строк: "имя_переменной: значение_переменной".
* _INFO_* - поля, заданные в разделе config.xml -> config -> point_info.
* _TERMNUMBER - номер терминала (config.xml -> config -> parameters -> point_id)
* _DATETIME - дата и время
* _ACCEPTED - количество принятых наличных денежных средств
* _COMISSION - комиссия
* _ENROLLED - количество денежных средств к зачислению на счёт клиента
* _CURRENCY - трехбуквенный код валюты платежа
* _OPNAME - имя оператора платежа

Только для чека инкассации:
* _INCS_CASSETTE - номер текущей кассеты с наличностью (в настоящее время заменяется на пустую строку)
* _INCS_NEXT_CASSETTE - номер новой кассеты (в настоящее время заменяется на пустую строку)
* _INCS_LAST_INCASSATION_TIME - предыдущее время инкассации
* _INCS_INCASSATION_TIME - время текущей инкассации
* _INCS_MONEY_INFO - подробная информация о принятых купюрах

## Логи (Logs/*.log)

Хранятся в директории logs, расположенной на одном уровне с приложением. Имя файла включает объект логгирования и дату. Каждая строка в файле состоит из "типа сообщения", времени, идентификатора потока, номера строки кода и текста сообщения.

Объекты логгирования:
* Kiosk - лог с основной информацией.
* Main - критическая информация о работе программы, в случае отсутствия ошибок пуст.
* Device_* - логи устройств, где * - имя устройства.
	
Тип сообщения:
* INF - простое, можно не обращать внимания, пока всё идет хорошо.
* WRN - необходимо обратить внимаение, возможно что-то пошло не так, как планировалось.
* ERR - произошла ошибка.
* EXT - расширенная информация, выводится только когда включена соответствующая настройка в конфиге киоска.
	
## Эмуляция устройств

В целях тестирования ПО вместо реальных устройств можно включать их эмуляцию. Для этого в настройках типа модели устройства необходимо указать специальное имя. Для эмуляции событий устройств нужно нажать на клавиатуре последовательно две клавиши из диапазона (F1 - F12). Первым нажатием выбирается устройство, вторым - событие данного устройства. 

* Validator.
    * model - "test_validator"
    * клавиша - "F9"
    * события:
        * F1 - устройство вышло из строя.
        * F2 - устройство работает.
        * F3 - кассета убрана.
        * F7 - внести 1 у.е. (1 копейка для RUB)
        * F8 - внести 10 у.е. (10 копеек для RUB)
        * F9 - внести 100 у.е. (1 рубль для RUB)
        * F10 - внести 500 у.е. (5 рублей для RUB)
        * F11 - внести 1000 у.е. (10 рублей для RUB)
        * F12 - внести 5000 у.е. (50 рублей для RUB)
		
* Printer - Печатает чеки в файлы (test_check_*.txt)
    * model - "test_printer"
    * клавиша - "F11"
    * события:
        * F1 - устройство вышло из строя.
        * F2 - устройство работает.
	
* Dispenser.
    * model - "test_dispenser"
    * клавиша - "F12"
    * события:
        * F1 - устройство вышло из строя.
        * F2 - устройство работает.
        * F3 - кассета убрана.
        * F4 - выдать колличество денег равное номиналу банкнот 0-ой кассеты.	

## Сервисные сочетания клавиш
* F1 + F1 - закрытие ПО Trovemat для доступа к системному меню.
* F1 + F2 - переход в сервисное меню ПО Trovemat.
* F1 + F3 - перезапуск ПО Trovemat без перезагрузки ОС.
* F1 + F4 - запуск утилиты калибровки сенсорного экрана.
* F9 + ... - виртуальный валидатор банкнот (купюроприёмник) - см. [Эмуляция устройств](#Эмуляция-устройств).
* F10 + ... - виртуальный сканер штрихкодов - см. [Эмуляция устройств](#Эмуляция-устройств).
* F11 + ... - виртуальный чековый принтер - см. [Эмуляция устройств](#Эмуляция-устройств).
* F12 + ... - виртуальный диспенсер купюр - см. [Эмуляция устройств](#Эмуляция-устройств).

## Особенности работы приложения в демонстрационном режиме
1. SMS отправляется через сервер [jetcrypto.com](https://jetcrypto.com/) от имени "Trovemat".
1. Платежи проходят через сервер [jetcrypto.com](https://jetcrypto.com/).
1. На экране поверх приложения выводится надпись о том что это демо-версия.
1. Приложение работает 10 минут, затем выключается (может быть увеличено по отдельному запросу на sales@trovemat.com).

## Особенности работы приложения
1. Номер телефона вводится в международном формате без символа "+" в начале и без кода страны. Конкретную страну необходимо выбирать из списка перед полем ввода номера телефона. Например для России надо будет вводить "9261234567".
1. На главной странице курсы берутся с сайта cryptocompare.com
1. Доступ к биржам осуществляется по токену, который надо прописывать в файле [configs/operators.xml](#Список-операторов-operatorsxml) для соответствующего поля request_field:
	1. BitLish: в шаге с id="0" и в шаге с id="7" у параметра "request_field" id="parameters.token" указывается токен для доступа к API.		
	1. EXMO: 
		- в шаге с id="0" у параметра "request_field" id="parameters.publicKey" указывает API key, в параметре "request_field" id="parameters.secretKey" указывается API secret.
		- в шаге с id="7" у параметра "request_field" id="params" в параметре указывается API key в атрибуте "publicKey" JSON-объекта, API secret указывается в атрибуте "secretKey" JSON-объекта.
	1. Poloniex: в шаге с id="7" у параметра "request_field" id="params" в параметре указывается POLONIEX API Key в атрибуте "publicKey" JSON-объекта, POLONIEX API Secret указывается в атрибуте "secretKey" JSON-объекта.

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
1. Перейти в сервисный режим (см. [Сервисные сочетания клавиш](#Сервисные-сочетания-клавиш))
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

1. Close Trovemat software (см. [Сервисные сочетания клавиш](#Сервисные-сочетания-клавиш))
1. Press ALT + F4
1. Press "Logout" button
1. When you're done with the command line - run command "exit" to launch Trovemat software or just simply reboot
