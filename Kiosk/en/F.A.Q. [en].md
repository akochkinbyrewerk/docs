# Frequently Asked Questions
## How does Trovemat Kiosk works?
       
Trovemat software steps:
- Accept cash
- Determine wallet address, where to transfer cryptocurrency:
    
    - Scan existing wallet address from QR-code (mobile phone, paper wallet, etc)
    - Enter existing wallet address using on-screen keyboard
    - Create new paper-wallet (if available) directly on kiosk, if the user doesn't have any wallet yet
- Print receipt to client (including paper-wallet, if it was created)

Watch video, showing Trovemat software example: https://youtu.be/6f3cR620obs

Technical information can be found at: https://trovemat.com

Install guide can be found here: ["Install Trovemat software guide"](https://github.com/trovemat/docs/blob/master/Kiosk/en/Install%20%5Ben%5D.md)
    
## Can you please describe client journey of the Trovemat software? How clients can buy cryptocurrency in Trovemat Kiosk?

Here are process of byuing cryptocurrency in Trovemat Kiosk step-by-step: 
1. Client insert cash into Kiosk (like in any other vending machines)
1. Client scans address of the recepient wallet (his wallet, or any other person), or enter wallet address using on-screen keyboard, or creates new paper-wallet by pressing corresponding button.
1. Client enter phone number for KYC procedure (code verification by SMS)
1. Trovemat software performs query to api.jetcrypto.com for SMS verification code (using phone number, provided by the client at previous step)
1. Client enters verification code, received by SMS
1. Trovemat software performs query to cryptocurrency exchange in order to transfer cryptocurrency from owner's wallet to client wallet
1. Cryptocurrency exchange performs withdrawal operation
1. Trovemat software prints receipt for the client with payment details (transferred amount, wallet, etc)
1. Client receives SMS with transactionId (if available) of the performed operation, which can be checked in the corresponding blockchain

## Can you describe the main flow-chart for a Trovemat software? How can you describe the flow of the money?

!["Scheme Trovemat flow of funds"](https://github.com/trovemat/docs/blob/master/Kiosk/en/img/Trovemat%20flow%20of%20funds.png)

1. Trovemat owner deposit his account on any of the supported cryptocurrency exchanges (it doesn't matter in which currency - crypto or fiat)
1. In owner's account on the cryptocurrency exchange owner must generate keys for accessing exchange API (for performing withdrawal operations)
1. Trovemat owner saves API keys in Trovemat software
1. Client inserts cash into Trovemat Kiosk
1. After KYC procedure and determining destination wallet address, Trovemat software sends a withdrawal command to cryptocurrency exchange, using destination wallet address.
1. Cryptocurrency exchange performs withdrawal command to destination wallet address
1. Trovemat owner performs money collection procedure from Kiosk when cashbox is filled up with fiat money

## Which cryptocurrency exchanges are supported by Trovemat software?
    
The list of supported cryptocurrency exchanges: 
- https://exmo.com
- https://poloniex.com
- https://bittrex.com
- https://bitfinex.com
- https://jetcrypto.com

This list is a subject to change upon request.

## Which cryptocurrency can I buy? Where the conversion rate is calculating?
   
You can buy any of the cryptocurrency, supported by concrete cryptocurrency exchange (from the list of supported exchanges). Conversion rates are setup by Trovemat software. Rates for displaying on the main page Trovemat software can get from concrete exchange (if available), or from aggregator such as coinmarketcap.com (or any other supported by Trovemat software).

## Trovemat client can top-up any wallet using Kiosk?

Yes, Trovemat client can define any valid wallet address for any cryptocurrency, supported by Trovemat software.

## Which experience Trovemat owner (or his employee) must have in order to maintain and support kiosk? Do I need to complete some courses for that?

Almost all configuration and support for the software can be done through TOX client (changing configuration files, viewing log files, updating software). Maintainer must understand the principles of modifying XML-files, and must be prepared to communicate with Trovemat software using any software, supported TOX protocol.

## What about power consumption of the kiosk?
    
Power consumption of the Kiosk: 200 Watts – 500 Watts, depending of the kiosk current state. For example: in-service mode (main screen displayed) - 200 Watts, all devices working (receipt printer, display, cash identification module) - 500 Watts.

## Which hardware is supported by Trovemat software?
    
Cash identification modules:
1. MEI Advance SC (RS-232 connection, using MEI EBDS communication protocol)

Receipt printers:
1. CUSTOM VKP-80II (USB / RS-232)
1. CUSTOM TG-2480 (USB / RS-232)
1. Nippon NP-F3092D

Touchscreen:
Any model, supported by OS.

## How can I pay for Trovemat Kiosk?

After you place your order on our web-site, we will contact with you about payment details. We require 100% payment of the ordeк before order will be processed.

## Delivery

We deliver Kiosks all over the world, delivery dates to your country depends on the presence of the Kiosks on the nearest warehouse. We can use any company by your choice to arrange the delivery. Delivery cost will be included in the invoice.

## What about warranty perios and software updates?
    
During warranty period (1 year after purchase) software updates is free (Trovemat Kiosk must have unlimited access to api.jetcrypto.com in order to perform automatic updates). After 1 year warranty can be extended, details available upon request on sales@trovemat.com
