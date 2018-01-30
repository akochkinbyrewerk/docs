
# Trovemat Kiosk User Guide

## Файлы настроек киоска (configs)
Файлы настроек киоска хранятся в директории configs.
* config.xml - настройки киоска.
* menu.xml - структура меню.
* operators.xml - список операторов.
* crypto.xml - список зашифрованных переменных.
* *_lastgood.xml - последний успешно загруженный файл.
* *_lastbad.xml - последний неудачно загруженный файл.
	
Пример:
1. Если config.xml - правильный. После успешной проверки киоск его скопирует и сохранит под именем config_lastgood.xml.
1. Если [operators.xml](#Список-операторов-operatorsxml) - неправильный. После неуспешной проверки киоск его скопирует и сохранит под именем operators_lastbad.xml. Далее попробует загрузить operators_lastgood.xml, и если он окажется валидным: скопирует operators_lastgood.xml и сохранит под именем [operators.xml](#Список-операторов-operatorsxml)

## Настройки киоска (config.xml)
	
Пример конфига:
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
            <args></args>
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

> Значения по умолчанию.
Если параметр в конфиге отсутствует или имеет значение "default" - берется его значение по умолчанию. 

> Ccылки на значения.
Если значение параметра начинается на "config." - киоск трактует его как ссылку. В примере выше атрибут extended_logging тега barcodereader задан через ссылку на значение extended_logging в parameters.

* parameters - общие параметры терминала.
    * extended_logging - дополнительное логирование:
   	 > **!!! ВНИМАНИЕ ! При включении дополнительного логгирования в лог-файлы будут попадать абсолютно все данные, передаваемые устройству, в том числе секретные данные, не предназначенные для логгирования !!!** 
		- **"false"** (значение по умолчанию) - отключено.
		- **"true"** - включено.
    * point_id - номер точки. Значение по умолчанию - 0.
    * point_name - наименование точки для отображения в мессенджере в качестве имени контакта. Если данный атрибут не указан - наименование контакта будет "Trovemat kiosk #<ЗНАЧЕНИЕ ИЗ АТРИБУТА point_id>".
* payments - параметры платежей по умолчанию. Могут быть переопределены для каждого оператора на шаге "money_entry" в operators.xml. Описание параметров в разделе "Операторы".
    * gateway - значение по умолчанию - "" - наименование шлюза
    * currency - значение по умолчанию - "USD".
    * limit_min - значение по умолчанию - "0".
    * limit_max - значение по умолчанию - "1000000".
    * payment_type - значение по умолчанию - "mut_price".
    * fee - по умолчанию - отсутствует.
    * check_name - значение по умолчанию - "default.chq".
    * failed_check_name - значение по умолчанию - "default_failed.chq".
    * common_params - список параметров, которые можно использовать в operators.xml подобно "полям".
* interface
    * wait_init_devices - переход со страницы инициализации на страницу меню.
		- **"0"** (значение по умолчанию) - не дожидаясь инициализации устройств.
		- **"1"** - ждать завершение инициализации устройств с параметром "show_errors" не равным "0".
		- **"2"** - ждать завершение инициализации всех устройств.
	* lang - язык интерфейса главной страницы по умолчанию
	* default_phone_code - код страны по-умолчанию по стандарту ISO 3166-1 при вводе номера телефона. Если значение атрибута пустое - код страны по-умолчанию отображаться не будет. Если данный атрибут не указан - код страны по-умолчанию будет "+1"
	* menu - тег, описывающий поведение главной страницы:
		* vending - значение по умолчанию "true" - флаг, разрешающий внесение наличных денежных средств на главном экране без выбора конкретной крипто-валюты для покупки
		* limit_max - максимальная сумма для внесения на главном экране в указанной валюте.
	* inactivity_timer - таймаут неактивности пользователя в секундах на различных экранах:
		* data_entry - на экране ввода (сканирования) номера кошелька
		* message - на экране сообщения клиенту
		* money_entry - на экране внесения наличных
* terminal
    * init - тег, описывающий параметры для запуска приложения при старте киоска
        * app - строка - приложение, которое киоск запустит отдельным процессом при старте.
        * args - строка - аргументы командной строки
        * timeout - целое число - время ожидания старта и завершения приложения в мсек. Если указано значение -1 - то таймаут не контролируется.
* peripherals    
    * model - модель устройства. Если устройства нет, оставить значение пустым. Значение по умолчанию - "".
    * port - номер последовательного порта, используемого данным устройством. Значение по умолчанию - "".
    * baudrate - скорость COM порта, используемого данным устройством. Значение по умолчанию - "9600".
    * extended_logging - дополнительное логирование, в том числе, всех команд передаваемых в COM-порт устройству.  
		> **!!! ВНИМАНИЕ ! При включении дополнительного логгирования в лог-файлы будут попадать абсолютно все данные, передаваемые устройству, в том числе секретные данные, не предназначенные для логгирования !!!**  
		- **"false"** (значение по умолчанию) - отключено.
		- **"true'** - включено.
    * show_errors - при обнаружении неисправностей в данном устройстве, показывать экран "Киоск не работает".
		- **"0"** (значение по умолчанию) - отключено.
		- **"1"** - только если нет активных платежей или оставшейся наличности.
		- **"2"** - переключать сразу.
* validator - Поддерживаемые модели (указывается в атрибуте model):
    - **"mei_ebds"** - купюроприёмник MEI. Поддерживается перепрошивка киоском. При запуске киоска проверяется наличие файла 'validator.bin' (прошивка) в корневой директории и, если таковой имеется, производится попытка прошивки. После успешной прошивки файл удаляется. Поддерживает следующие параметры:
		- **enabled_currencies** - список валют, которые может принимать купюроприёмник. Пустое значение означает приём всех валют, которые поддерживаются прошивкой купюроприёмника. Значение по умолчанию - пустая строка. Поддерживается 2 формата записи:
			1. Строка с разделителем "," (запятая), в строке через разделитель указываются трёхбуквенные наименования валют по стандарту ISO-4217. Пример: "RUB,USD" - разрешён приём валют "Российский рубль" (RUB, 643), "Доллар США" (USD, 840).
			1. Строка с разделителем "," (запятая), в строке через разделитель указываются конкретные банкноты, приём которых разрешён. Банкноты указываются в следующем формате "CUR:NOM", CUR - трёхбуквенное наименование валюты банкноты по стандарту ISO-4217, NOM - номинал указанной банкноты в основных единицах указанной валюты. Пример: "RUB:10,RUB:1000,USD:10,USD:100" - разрешён приём банкнот "10 Российских рублей" (RUB, 643, 10RUB), "1000 Российских рублей" (RUB, 643, 1000RUB), "10 долларов США" (USD, 840, 10USD), "100 долларов США" (USD, 840, 100USD).
    - **"test_validator"** - тестовый программно-эмулируемый купюроприёмник для отладочных целей.
* printer - Поддерживаемые модели (указывается в атрибуте model):
	- **"tg2480"** - чековый принтер CUSTOM TG2480. В некоторых случаях (зависит от версии прошивки ПО принтера) будет также поддерживаться принтер Custom VKP 80 II с такими же настройками.
	- **"np-f3092d"** - Чековый принтер Nippon NP-F3092D. Для данного принтера не реализована функция авто-поиска - параметры для подключения необходимо указывать вручную.
	- **"test_printer"** - тестовый программно-эмулируемый принтер для отладочных целей.
    * charset_code_table - номер таблицы символов, используемой при печати. Значение по умолчанию - "0".
        * Для tg2480:
            - **"0"** - CP437 (US)
            - **"2"** - CP850 (Multilingual)
            - **"3"** - CP860 (Portuguesse)
            - **"4"** - CP863 (Canadian-French)
            - **"5"** - CP865 (Nordic)
            - **"17"** - CP866 (Cyrillic)
            - **"19"** - CP858 (for Euro symbol 213)
* dispenser - Поддерживаемые модели (указывается в атрибуте model):
    - **"puloon_lcdm"** - диспенсер купюр Poloon LCDM1000/2000 (1 и 2 кассеты соответственно).
    - **"test_dispenser"** - тестовый программно-эмулируемый диспенсер купюр для отладочных целей (2 кассеты).
    * Следующих атрибутов необходимо задать столько же, сколько кассет содержит диспенсер:
    * cassette_0 - номинал купюр 0-ой кассеты, например "100 USD".
    * default_capacity_0 - количество купюр по умолчанию для 0-ой кассеты (при инкассации можно изменить это значение), например "1000".
		
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

## Список операторов (operators.xml)

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

## Настройка взаимодействия с биржами

### [Poloniex](https://poloniex.com)

API Key хранится в параметре "config.payments.common_params.poloniex_public_key"

Secret хранится в параметре "crypto.poloniex_secret_key"

### [Bitfinex](https://www.bitfinex.com/)

API key хранится в параметре "config.payments.common_params.bitfinex_public_key"

API key secret хранится в параметре "crypto.bitfinex_secret_key"

### [Bittrex](https://bittrex.com)

API key хранится в параметре "config.payments.common_params.bittrex_public_key"

API key secret хранится в параметре "crypto.bittrex_secret_key"

### [EXMO](https://exmo.com)

Public key хранится в параметре "config.payments.common_params.exmo_public_key"

Secret key хранится в параметре "crypto.exmo_secret_key"

## Запуск командной строки

1. Закрыть ПО Trovemat (см. [Сервисные сочетания клавиш](#Сервисные-сочетания-клавиш))
1. Нажать ALT + F4
1. В появившемся диалоге нажать кнопку "Выйти" ("Logout")
1. После завершения работы в консольном режиме наберите команду "exit" для запуска ПО Trovemat или перезагрузите ОС