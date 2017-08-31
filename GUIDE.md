# Using the GloBee Python Client Library


## Prerequisites
You must have GloBee or test.globee merchant account to use this library. [Signing up for a merchant account](https://globee.com/register) is free.

## Quick Start
### Installation

GloBee's python library was developed in Python 3.4.2. The recommended method of installion is using pip.

`pip install 'globee-py2'`

### Basic Usage

The globee library allows authenticating with GloBee, creating invoices, and retrieving invoices.
  
#### Pairing with GloBee.com

Before pairing with GloBee.com, you'll need to log in to your GloBee account and navigate to /api-tokens. Generate a new pairing code and use it in the next step. You can try out various functions using the Python REPL. In this example, it's assumed that we are working against the globee test server and have generated the pairing code "abcdefg".

    > from bitpay.bitpay_client import Client
    > client = Client(api_uri="https://test.globee.com") #if api_uri is not passed, it defaults to "https://globee.com"
    > client.pair_pos_client("abcdefg")

To perform a second pairing method - client-side pairing - you will need to specify the facade you want to create and call create_token(). The JSON response will include a pairing code to append to the end of the API access request URL specified in the documentation.  For example, if you wanted to create a token for the merchant facade:

    > from bitpay.bitpay_client import Client
    > client = Client(api_uri="https://test.globee.com")
    > client.create_token("merchant")

The response will include the pairing code which you'll use for the access request URL:

```
{'data': [{'pairingExpiration': 1426204460704, 'dateCreated': 1426118060704, 'pairingCode': 'AAAAAA',
'token': 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx', 'policies': [{'policy': 'id', 'params':
['xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'], 'method': 'inactive'}], 'facade': 'merchant'}]}
```

So in the example above, the URL would look like this: http://test.globee.com/api-access-request?pairingCode=AAAAAA

When you nagivate to this address, you will be prompted to approve this request.  Once approved, you can use the token returned to perform any API calls needed.  To learn more about this method, see the [GloBee REST API documentation](https://globee.com/api-docs#facades) for details.

#### To create an invoice with a paired client:

Using the same web client from the last step:

    > client.create_invoice({"price": 20, "currency": "USD", "token": client.tokens['pos']})

That will return the invoice as JSON. Other parameters can be sent, see the [GloBee REST API documentation](https://globee.com/api-docs#resource-Invoices) for details.

