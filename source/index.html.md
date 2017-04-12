---
title: API Reference

language_tabs:
  - shell: SHEll
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---


# EMVPadReset

cURL


Example request using the cURL utility.This example assumes there is a file called emv_test_request.xml in the active directory which contains the text of an XML transaction request:

```shell
curl - X POST https: //trancloud.dsipscs.com/ProcessEMVTransaction ^
--data @emv_test_request.xml ^
--basic--user "TestUser:TestAuthCode48446" ^
--header "UserTrace: testing with curl" ^
--header "Content-Type: application/xml"
```

The same example with a JSON request format with file test_request.json:

```shell
curl - X POST https: //trancloud.dsipscs.com/ProcessEMVTransaction ^
--data @emv_test_request.json ^
--basic--user "TestUser:TestAuthCode48446" ^
--header "UserTrace: testing with curl" ^
--header "Content-Type: application/json"
```

Note that the carrot( ^ ) symbol is the new - line escape character only
for Windows;
if you are using a bash shell on Linux or Unix, you would use a backslash(\) in place of the carrot symbols.

AJAX / jQuery

Example using AJAX with jQuery:

Apple
:   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.

Orange
:   The fruit of an evergreen tree of the genus Citrus.

<pre class="javascript tab-javascript">
  <code>
    ```
      <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>  

      <script type="text/javascript">
        $(document).ready(function() {
          $.support.cors = true;
        }

        function testTransactionJSON() {
          var endpoint = "https://trancloud.dsipscs.com/ProcessEMVTransaction";
          var accountId = "TestUser";
          var authCode = "TestAuthCode48446";

          var transRequest = new Object();
          transRequest['TStream'] = new Object();
          transRequest['TStream']['Transaction'] = new Object();

          var trs = transRequest['TStream']['Transaction'];
          trs['MerchantID'] = "003503902913105";
          trs['OperatorID'] = "TEST";
          trs['TranCode'] = "EMVSale";
          trs['InvoiceNo'] = "100";
          trs['RefNo'] = "100";
          trs['SecureDevice'] = "CloudEMV2";
          trs['RecordNo'] = "RecordNumberRequested";
          trs['Frequency'] = "OneTime";

          trs['Amount'] = new Object();
          trs['Amount']['Purchase'] = "1.25";
          trs['Amount']['Gratuity'] = "0.75";

          trs['SequenceNo'] = "0010010010";

          trs['TranDeviceID'] = "PT9999999999";
          trs['PinPadIpAddress'] = "10.0.0.155";
          trs['PinPadIpPort'] = "12000";

          trs['UserTrace'] = "my user trace";

          var headers = new Object();
          headers['Authorization'] = "Basic " + btoa(accountId + ":" + authCode);

          $.ajax({
            url: endpoint,
            type: "POST",
            headers: headers,
            contentType: "application/json; charset:utf-8;",
            data: JSON.stringify(transRequest),
            error: function(xhr, textStatus, errorThrown) {
              // handle error
            },
            dataType: "json"
          }).done(function(response) {
            var rst = response['RStream'];
            alert("Response Origin:  " + rst['ResponseOrigin'] + "\nReturn code:  " + rst['DSIXReturnCode'] + "\nStatus:  " + rst['CmdStatus'] + "\nText Response:  " + rst['TextResponse'] + "\nUser Trace:  " + rst['UserTrace']);
          });
        } 
      </script>
    ```
  </code>
</pre>

```javascript
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>  

<script type="text/javascript">
  $(document).ready(function() {
    $.support.cors = true;
  }

  function testTransactionJSON() {
    var endpoint = "https://trancloud.dsipscs.com/ProcessEMVTransaction";
    var accountId = "TestUser";
    var authCode = "TestAuthCode48446";

    var transRequest = new Object();
    transRequest['TStream'] = new Object();
    transRequest['TStream']['Transaction'] = new Object();

    var trs = transRequest['TStream']['Transaction'];
    trs['MerchantID'] = "003503902913105";
    trs['OperatorID'] = "TEST";
    trs['TranCode'] = "EMVSale";
    trs['InvoiceNo'] = "100";
    trs['RefNo'] = "100";
    trs['SecureDevice'] = "CloudEMV2";
    trs['RecordNo'] = "RecordNumberRequested";
    trs['Frequency'] = "OneTime";

    trs['Amount'] = new Object();
    trs['Amount']['Purchase'] = "1.25";
    trs['Amount']['Gratuity'] = "0.75";

    trs['SequenceNo'] = "0010010010";

    trs['TranDeviceID'] = "PT9999999999";
    trs['PinPadIpAddress'] = "10.0.0.155";
    trs['PinPadIpPort'] = "12000";

    trs['UserTrace'] = "my user trace";

    var headers = new Object();
    headers['Authorization'] = "Basic " + btoa(accountId + ":" + authCode);

    $.ajax({
      url: endpoint,
      type: "POST",
      headers: headers,
      contentType: "application/json; charset:utf-8;",
      data: JSON.stringify(transRequest),
      error: function(xhr, textStatus, errorThrown) {
        // handle error
      },
      dataType: "json"
    }).done(function(response) {
      var rst = response['RStream'];
      alert("Response Origin:  " + rst['ResponseOrigin'] + "\nReturn code:  " + rst['DSIXReturnCode'] + "\nStatus:  " + rst['CmdStatus'] + "\nText Response:  " + rst['TextResponse'] + "\nUser Trace:  " + rst['UserTrace']);
    });
  } 
</script>
```


Use: To reset and ready the EMV PIN pad device for a new transaction.

Notes: This command must be performed before every transaction (Sale, VoidSale, Return, VoidReturn, VoiceAuth and ZeroAuth) to assure that no card is in the EMV PIN pad chip card (insertion) reader before starting a new transaction.  This command should also be performed at the end of any card related transaction (Sale, VoidSale, Return, VoidReturn, VoiceAuth and ZeroAuth) to assure that the user is prompted to remove their card.  If no EMV chip card is inserted in the reader, then an EMVPadReset will return a successful response in one second.  If there’s an EMV chip card in the reader, then the EMVPadReset command will wait up to 15 seconds for removal.  If the card is removed within that timeframe, then a successful response is returned when the card is removed. If the chip card remains in the reader after 15 seconds, an error response is returned "TRANSACTION NOT COMPLETE - Card Not Removed".

XML Request Template:

Field values in BOLD are required.  Field values in LIGHT GRAY are optional and extend the functionality of the basic transaction.  See the following XML element table for use of both required and optional fields.

```xml
<?xml version=“1.0”?>
<TStream>
  <Transaction> 
    <MerchantID>MerchantID</MerchantID>
    <TerminalID>TerminalID</TerminalID>
    <OperatorID>OperatorID</OperatorID>
    <UserTrace>UserTrace</UserTrace>
    <TranCode>EMVPadReset</TranCode>
    <SecureDevice>SecureDevice</SecureDevice>
    <SequenceNo>SequenceNo</SequenceNo>
    <TranDeviceID>TranDeviceID</TranDeviceID>
    <PinPadIpAddress>PinPadIpAddress</PinPadIpAddress>
    <PinPadMACAddress>PinPadMACAddress</PinPadMACAddress>
    <PinPadIpPort>PinPadIpPort</PinPadIpPort>
  </Transaction>
</TStream>
```



Element | Req | Min | Max | Type | Description/Value
-- | -- | -- | -- | -- | --
MerchantID | Y | 1 | 24 | A | Merchant identification assigned by processor.
TerminalID | Y | 1 | 24 | A | Terminal ID data must be supplied in this tag only if required by the processor or merchant service provider; otherwise this tag should not be included.  The POS system should store this value as a parameter to be included when required.
OperatorID | O | 1 | 24 | A | Operator (clerk, server, etc.) associated with the transaction.
UserTrace | O | 1 | 24 | A | A unique value created and supplied by POS system.
TranCode | Y | 1 | 40 | A | “EMVPadReset”
SecureDevice | Y | 1 | 24 | A | “CloudEMV2” or a specific value as shown in Section 1.4
SequenceNo | Y | 10 | 12 | A | Use the value returned for <SequenceNo> from the last response.  If the SequenceNo is lost or for initial deployment of the PIN pad, the ECR/POS should attempt any transaction (using “0010010010” as a SequenceNo value) to re-sync.
TranDeviceID | O | 10 | 10 | A | The tran device ID in the format PT9999999999. This must be given either in the request body or as an HTTP header.
PinPadIpAddress | O | 7 | 15 | A | IP (v4) address of PIN pad on local network.
PinPadMACAddress | O | 12 | 17 | A | MAC (Media Access Control) address of PIN pad; delimited by colons, hyphens, or with no delimiter.
PinPadIpPort | O | 1 | 5 | N | IP port of PIN pad. Defaults to 12000.

Legend: A Alphanumeric    N Numeric
    O Optional    R Required


#### Sample EMVPadReset Request

Using PinPadIpAddress:

```xml--PinPadIpAddress
<?xml version="1.0"?>
<TStream>
  <Transaction>
    <MerchantID>700000000245</MerchantID>
    <TerminalID>001</TerminalID>
    <OperatorID>TEST</OperatorID>
    <UserTrace>Dev1</UserTrace>
    <TranCode>EMVPadReset</TranCode>
    <SecureDevice>CloudEMV2</SecureDevice>
    <SequenceNo>0010010010</SequenceNo>
    <TranDeviceID>PT9999999999</TranDeviceID>
    <PinPadIpAddress>10.0.0.155</PinPadIpAddress>
    <PinPadIpPort>12000</PinPadIpPort>
  </Transaction>
</TStream>
```

Using PinPadMACAddress:

```xml--PinPadMACAddress
<?xml version="1.0"?>
<TStream>
  <Transaction>
    <MerchantID>700000000245</MerchantID>
    <TerminalID>001</TerminalID>
    <OperatorID>TEST</OperatorID>
    <UserTrace>Dev1</UserTrace>
    <TranCode>EMVPadReset</TranCode>
    <SecureDevice>CloudEMV2</SecureDevice>
    <SequenceNo>0010010010</SequenceNo>
    <TranDeviceID>PT9999999999</TranDeviceID>
    <PinPadMACAddress>54:E1:40:13:57:AF</PinPadMACAddress>
    <PinPadIpPort>12000</PinPadIpPort>
  </Transaction>
</TStream>
```

Sample EMVPadReset Response

```xml
<?xml version="1.0"?>
<RStream>
<ResponseOrigin>Client</ResponseOrigin>
<DSIXReturnCode>000000</DSIXReturnCode>
<CmdStatus>Success</CmdStatus>
<TextResponse>Reset Successful.</TextResponse>
<SequenceNo>0010010010</SequenceNo>
<UserTrace>Dev1</UserTrace>
</RStream>
```

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

