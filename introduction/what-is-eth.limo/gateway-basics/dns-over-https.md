# DNS over HTTPS

**Endpoint**

eth.limo provides an RFC 8484 comptaible public DoH API which can be accessed at this endpoint:

* [https://dns.eth.limo/dns-query](https://dns.eth.limo/dns-query)

Currently we only support DNSLink `TXT` records due to client limitations for handling other record types which are dervied from an ENS contenthash.

**What is DNSLink?**

DNSLink provides an application friendly specification for linking content between services. Please refer to [https://dnslink.dev/](https://dnslink.dev/) for a detailed explanation.

***

## Response formats

The eth.limo DoH resolver supports both JSON and DNS Wireformat responses. Please see below for more information.

### JSON

`GET`

The `GET` verb is reserved for use with JSON replies only. The URL must be constructed with these query strings:

* `name=` (Required) - ENS domain name to resolve, i.e. `vitalik.eth`
* `type=TXT` (Required) - At the moment the only supported value for the `type=` parameter is `TXT`
* `dns=<base64 encoded DNS message>` (Optional) - Send a base64 encoded DNS message instead of relying upon `name`.

An example query would be:

```shell
$ curl 'https://dns.eth.limo/dns-query?name=vitalik.eth&type=TXT'
```

Which returns the following payload:

```json
{
  "Status": "0",
  "RD": false,
  "RA": false,
  "AD": false,
  "CD": false,
  "TC": false,
  "Question": [
    {
      "type": 16,
      "name": "vitalik.eth"
    }
  ],
  "Answer": [
    {
      "type": 16,
      "name": "vitalik.eth",
      "data": "dnslink=/ipfs/QmQhCuJqSk9fF58wU58oiaJ1qbZwQ1eQ8mVzNWe7tgLNiD/",
      "ttl": 600
    }
  ]
}
```

The content path is available in `.Answer[0].data`.

### DNS Wireformat

`POST`

If your application needs support for binary data, please use the `POST` method along with the `Content-Type: application/dns-message` header in your request. All data must be binary encoded. Please refer to the documentation of your DoH client for more information on how to properly construct requests.

In this example, we'll use [https://github.com/curl/doh](https://github.com/curl/doh) as our client.

```shell
$ doh -tTXT vitalik.eth https://dns.eth.limo/dns-query
[vitalik.eth]
TTL: 600 seconds
TXT: dnslink=/ipfs/QmQhCuJqSk9fF58wU58oiaJ1qbZwQ1eQ8mVzNWe7tgLNiD/

```

### .ART Support

Our DoH API also supports `.art` domains. For example:

```shell
$ curl 'https://dns.eth.limo/dns-query?name=limo.art&type=TXT'

{"Status":"0","RD":false,"RA":false,"AD":false,"CD":false,"TC":false,"Question":[{"type":16,"name":"limo.art"}],"Answer":[{"type":16,"name":"limo.art","data":"dnslink=/ipfs/bafybeico3uuyj3vphxpvbowchdwjlrlrh62awxscrnii7w7flu5z6fk77y/","ttl":600}]}
```

***

### HTTP Response Codes

`200` - The request was processed successfully

`422` - We don't support the contenthash type or are otherwise unable to process the request.

`451` - The content is unavailable due to legal reasons.

`5xx` - Something catastrophic has happened.
