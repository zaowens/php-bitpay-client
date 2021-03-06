# Creating BitPay invoices - the tutorial
==========================

## About this tutorial
This tutorial contains four scripts. These scripts allow you to do the following:
1) Create keys to communicate with BitPay's API
2) Pair your keys to your BitPay merchant account
3) Create BitPay invoices
4) Log IPNs (Instant Payment Notifications or webhooks)

Script 001 & 002 need to be executed once, to properly configure your local installation.

Script 003 creates BitPay invoices; this script can be run permanently.

IPNs will be sent after a BitPay invoice receives a payment. IPNs can be logged or processed with IPNlogger.php

## Getting started
To begin please visit https://test.bitpay.com/dashboard/signup and register for a BitPay merchant test account. Please fill in all questions, so you get a fully working test account.

If you are looking for a testnet bitcoin wallet to test with, please visit https://bitpay.com/wallet and
create a new wallet.

If you need testnet bitcoin please visit a testnet faucet, e.g. https://testnet.coinfaucet.eu/en/ or http://tpfaucet.appspot.com/

For more information about testing, please see https://bitpay.com/docs/testing

Please make sure to use BitPay's latest PHP library (bitpay/php-client)

## Script 1 & 2: configure your local installation
The following two scripts need to be executed once. These scripts will generate your private/public keys and pair them to your BitPay merchant account:
1. 001_generateKeys.php : generates the private/public keys to sign the communication with BitPay. The private/public keys are stored in your filesystem for later usage.
2. 002_pair.php : pairs your private/public keys to your BitPay merchant account. Please make sure to first create a pairing code in your BitPay merchant account (Payment Tools -> Manage API tokens) and put this pairing code in the script. The script returns an API token that should be put put in 003_createInvoice.php, to create invoices permanently.

These first two scripts need to be executed only once.

## Script 3: create a BitPay invoice
3. 003_createInvoice.php : creates a BitPay invoice. Please make sure to update the script with the API token received from 002_pair.php

This script returns a BitPay invoice object. You can display the invoice by loading the invoice-URL in a web browser. You can pay the invoice with your bitcoin wallet.

For more information about paying a BitPay invoice, please see http://help.bitpay.com/paying-with-bitcoin/order-information-and-assistance/how-do-i-pay-a-bitpay-invoice-using-bitcoin

This script can be run permanently.

## Script 4: log IPNs
After you've paid the invoice, BitPay will send an IPN to the notificationURL of the invoice. This script should be put on your server and be reachable from the internet. Your should put the URL of IPNLogger.php in 003_createInvoice.php, e.g.:
```
// You will receive IPN's at this URL, should be HTTPS for security purposes!
$invoice->setNotificationUrl('https://yourserver.com/IPNlogger.php');
```
IPNs can be used by the merchant to update their order status. Please note to use IPNs as a trigger to fetch the BitPay invoice status, since the IPNs are not authenticated.

For more information about IPNs, please see https://bitpay.com/docs/invoice-callbacks


Examples (c) 2014-2017 BitPay
