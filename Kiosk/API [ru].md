# Формат и последовательность запросов к https://beta.jetcrypto.com для выполнения платежей

Во всех ответах (если в описании ответа не указано иное) возвращается JSON-строка следующего формата:
```JSON
{
	"status":0,
	"errors":null,
	"data":{}
}
```
* "status" -целое число, сигнализирует о наличии ошибки. "0" - ошибки нет, другое значение - ошибка.
* "errors" - массив ошибок (если есть). Если ошибок нет - значение null.
* "data" - данные ответа, формат зависит от запроса.
При описании запросов в описании форматов ответов описывается структура данных из элемента "data". 

1. Авторизация (обязательный запрос): https://beta.jetcrypto.com/api/token
    1. Параметры POST-запроса: 
	    * "username" - имя пользователя
	    * "password" - пароль пользователя
    1. Ответ (возвращается в основном объекте на том же уровне что и "errors"):
	    * "access_token" - токен, который надо будет передавать в последующих запросах.
	    * "expires_in" - таймаут токена в секундах. После истечения этого промежутка времени токен будет невалидный, его надо будет запрашивать заново. Данный токен необходимо будет включать во все последующие запросы в заголовок "Authorization" в формате "Bearer <ЗНАЧЕНИЕ ПОЛУЧЕННОГО ТОКЕНА>".
    1. Пример POST-запроса (указан ВЕСЬ текст запроса включая HTTP заголовок): 
        ```
        POST /api/token HTTP/1.1
        Host: beta.jetcrypto.com
        Accept: */*
        Content-Type: application/json
        Connection: close
        Content-Length: 37
        {"username":"demo","password":"demo"}
        ```
    1. Пример ответа:
        ```
        HTTP/1.1 200 OK
        Server: nginx
        Date: Tue, 12 Sep 2017 08:48:42 GMT
        Content-Type: application/json
        Transfer-Encoding: chunked
        Connection: close
        Vary: Accept-Encoding
        X-Frame-Options: SAMEORIGIN
        X-XSS-Protection: 1; mode=block
        {
          "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJkZW1vIiwianRpIjoiYzA5Y2M4MTQtOWY2My00ODFhLWJlMDctMmJiODZiMjA4MTY4IiwiaWF0IjoxNTA1MjA2MTIyLCJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiMTM2IiwibmJmIjoxNTA1MjA2MTIyLCJleHAiOjE1MDUyMDk3MjIsImlzcyI6IkRlbW9Jc3N1ZXIiLCJhdWQiOiJEZW1vQXVkaWVuY2UifQ.zmI0suMxhIaflYK1tQFUfKm7m_JDFk_FdONOfyBFCIM",
          "expires_in": 3600
        }
        ```
1. Запрос курса (необязательный запрос) -  https://beta.jetcrypto.com/api/trovemat/Payment/getData
    Получение курса конвертации. В начале мы запрашиваем курс конвертации USDT -> BTC чтобы можно было клиенту отобразить курс биржи, если принимаем доллары.
    1. Параметры POST-запроса:
        * "operatorId" - идентификатор биржи, для Poloniex необходимо указывать значение "31"
        * "request" - идентификатор операции, для проверки курса необходимо указывать "term_rate"
        * "parameters" - строка с параметрами, которые будут отправлены на биржу. Для Poloniex необходимо указывать строку в формате JSON: {"pair":"<НАИМЕНОВАНИЕ ПАРЫ ВАЛЮТ ДЛЯ КОТОРЫХ НАДО ПРОВЕРИТЬ КУРС>"}
    1. Ответ:
        * "lowestAsk" - самое дешёвое предложение по продаже данной пары (т.е. та минимальная цена, по которой можно купить данную пару).
        * "highestBid" - самое дорогое предложение по покупке данной пары (т.е. та максимальная цена, по которой можно продать данную пару).
    1. Пример POST-запроса (указан ВЕСЬ текст запроса включая HTTP заголовок): 
        ```
        POST /api/trovemat/Payment/getData HTTP/1.1
        Host: beta.jetcrypto.com
        Accept: */*
        Content-Type: application/json
        Connection: close
        Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJkZW1vIiwianRpIjoiYzA5Y2M4MTQtOWY2My00ODFhLWJlMDctMmJiODZiMjA4MTY4IiwiaWF0IjoxNTA1MjA2MTIyLCJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiMTM2IiwibmJmIjoxNTA1MjA2MTIyLCJleHAiOjE1MDUyMDk3MjIsImlzcyI6IkRlbW9Jc3N1ZXIiLCJhdWQiOiJEZW1vQXVkaWVuY2UifQ.zmI0suMxhIaflYK1tQFUfKm7m_JDFk_FdONOfyBFCIM
        Content-Length: 74
        {"operatorId":"31","parameters":{"pair":"USDT_BTC"},"request":"term_rate"}
        ```
    1. Пример ответа:
        ```
        HTTP/1.1 200 OK
        Server: nginx
        Date: Tue, 12 Sep 2017 08:48:43 GMT
        Content-Type: application/json; charset=utf-8
        Transfer-Encoding: chunked
        Connection: close
        Vary: Accept-Encoding
        Cache-Control: private, no-cache, must-revalidate, max-age=3600
        X-Frame-Options: SAMEORIGIN
        X-XSS-Protection: 1; mode=block
        {"status":0,"errors":null,"data":{"id":121,"last":"4304.55296992","lowestAsk":"4304.55296988","highestBid":"4300.64503227","percentChange":"0.03351844","baseVolume":"21215974.57295936","quoteVolume":"4995.42522410","isFrozen":"0","high24hr":"4376.40009654","low24hr":"4155.00000007"}}
	    ```
1. GET-Запрос на отправку SMS-кода для KYC при покупке крипто-валюты (необязательный запрос): https://beta.jetcrypto.com/api/trovemat/Phone/verify
    При выполнении данного GET-запроса параметры кодируется в URL запроса. 
    1. Параметры запроса:
        * "message" - текст SMS, который будет отправлен клиенту. Текст может содержать строку '{0}' - вместо этих 3-х символов при отправке СМС будет подставлен 8-ми значный случайно сгенерованный цифровой код.
        * "phoneNumber" - номер телефона в международном формате (может отсутствовать знак '+' перед кодом страны).
    1. Ответ (возвращается в основном объекте на том же уровне что и "errors"):
        * "verifyCode" - HASH кода подтверждения, отправленного на указанный номер мобильного телефона.
    1. Пример GET-запроса:
        ```
        http://beta.jetcrypto.com/api/trovemat/Phone/verify?message=Trovemat%20verification%20code%3A%20%7B0}&phoneNumber=79262234733
        ```
    1. Полный текст примера запроса:
        ```
        GET /api/trovemat/Phone/verify?message=Trovemat%20verification%20code%3A%20%7B0}&phoneNumber=79262234733 HTTP/1.1
        Host: beta.jetcrypto.com
        Accept: */*
        Content-Type: application/x-www-form-urlencoded
        Connection: close
        Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJkZW1vIiwianRpIjoiYzA5Y2M4MTQtOWY2My00ODFhLWJlMDctMmJiODZiMjA4MTY4IiwiaWF0IjoxNTA1MjA2MTIyLCJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiMTM2IiwibmJmIjoxNTA1MjA2MTIyLCJleHAiOjE1MDUyMDk3MjIsImlzcyI6IkRlbW9Jc3N1ZXIiLCJhdWQiOiJEZW1vQXVkaWVuY2UifQ.zmI0suMxhIaflYK1tQFUfKm7m_JDFk_FdONOfyBFCIM 
        ```
    1. Пример ответа:
        ```
        HTTP/1.1 200 OK
        Server: nginx
        Date: Tue, 12 Sep 2017 08:49:19 GMT
        Content-Type: application/json; charset=utf-8
        Transfer-Encoding: chunked
        Connection: close
        Vary: Accept-Encoding
        Cache-Control: public, no-cache, must-revalidate, max-age=3600
        X-Frame-Options: SAMEORIGIN
        X-XSS-Protection: 1; mode=block
        Debug-Cache-Status: MISS
        {"verifyCode":"Nv+K20XDrBdo0dbjRGmuLUcainemC5wj4Yb7CXsMDaQ=","errorCode":0,"errors":{}} 
        ```
1. POST-запрос - осуществление перевода средств с кошелька владельца терминала на кошелёк, указанные клиентом.
    1. Параметры запроса:
        * "amount" - сумма списания в валюте наличных, которые клиент внёс в киоск (в минимальных единицах валюты).
        * "currencyId" - код внесённой валюты по стандарту ISO-4217
        * "operatorId" - идентификатор оператора. Для Polonies необходимо указывать "31".
        * "params" - строка с параметрами операции. Параметры записываются в виде JSON-объекта.
            Формат объекта, который передаётся в строке "params":
            * "currency" - идентификатор крипто-валюты для перечисления на кошелёк клиента.
            * "address" - адрес кошелька, на который необходимо перевести крипто-валюту.
            * "secretKey" - Секретный ключ кошелька с которого переводятся деньги, для передачи бирже Poloniex.
            * "publicKey" - Публичный ключ кошелька с которого переводятся деньги, для передачи бирже Poloniex.
            * "withdrawalAmount" - сумма крипто-валюты для перечисления.
    1. Ответ:
        * "errorCode" - идентификатор ошибки. 0 - ошибки нет, любое другое значение - код ошибки.
    1. Пример POST-запроса:
        ```
        POST /api/trovemat/Payment HTTP/1.1
        Host: beta.jetcrypto.com
        Accept: */*
        Content-Type: application/json
        Connection: close
        Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJkZW1vIiwianRpIjoiYTQxN2EwMDktNjhjNy00OWVlLTlkODgtNGNlMDE3YzA0Y2VhIiwiaWF0IjoxNTA1NzI0Mjk5LCJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiMTM2IiwibmJmIjoxNTA1NzI0Mjk5LCJleHAiOjE1MDU3Mjc4OTksImlzcyI6IkRlbW9Jc3N1ZXIiLCJhdWQiOiJEZW1vQXVkaWVuY2UifQ.r8-iqpUTbH_nqlYGwxfytqXs5pJgeSHxDJdr04TKrsQ
        Content-Length: 369
        {"amount":"0","currencyId":"840","operatorId":"31","params":"{\"currency\":\"BTC\",\"address\":\"1qqwerrttttttttttttttttttttttt\",\"secretKey\":\"a58e8f5ac3f8aef2e5fcce9d53da8fe2c73fabdf5c691d2b237a6df6fb5ecdbccb2a6b2c93f248b3de2ec1d39d85ba9f6e5955541198221e2ce02b5d155caf56\",\"publicKey\":\"L8JEFBDI-46LKSE9Q-5W80NV1L-9N9EM5CX\",\"withdrawalAmount\":\"0.00025413\"}"}
        ```        
    1. Пример ответа:
        ```
        HTTP/1.1 200 OK
        Server: nginx
        Date: Mon, 18 Sep 2017 08:46:12 GMT
        Content-Type: application/json; charset=utf-8
        Transfer-Encoding: chunked
        Connection: close
        Vary: Accept-Encoding
        Cache-Control: private, no-cache, must-revalidate, max-age=3600
        X-Frame-Options: SAMEORIGIN
        X-XSS-Protection: 1; mode=block
        {"id":33,"operatorId":31,"statusId":3,"createDate":"2017-09-18T08:46:11.5515558Z","completeDate":null,"params":"{\"currency\":\"BTC\",\"address\":\"1qqwerrttttttttttttttttttttttt\",\"secretKey\":\"a58e8f5ac3f8aef2e5fcce9d53da8fe2c73fabdf5c691d2b237a6df6fb5ecdbccb2a6b2c93f248b3de2ec1d39d85ba9f6e5955541198221e2ce02b5d155caf56\",\"publicKey\":\"L8JEFBDI-46LKSE9Q-5W80NV1L-9N9EM5CX\",\"withdrawalAmount\":\"0.00025413\"}","currencyId":840,"amount":0,"userId":136,"moneySource":0,"moneySourceId":-1,"session":"242f29fcf06e4523b3b421ea00420635","errorCode":1,"errors":null}
        ```
