# Using eth.limo with IPFS (Kubo)

The most popular IPFS client, [Kubo](https://github.com/ipfs/go-ipfs) (formally go-ipfs) can be configured to use eth.limo DNS for native ENS resolution by running the following command:

```shell
$ ipfs config --json DNS.Resolvers '{"eth.": "https://dns.eth.limo/dns-query"}'
```

Verify that the changes have been saved:

```shell
$ cat ~/.ipfs/config | jq .DNS
{
  "Resolvers": {
    "eth.": "https://dns.eth.limo/dns-query"
  }
}

```

In order for the changes to take effect, you must restart the IPFS process.

ENS names must be prefixed with `/ipns/` in order to successfully resolve them:

```shell
$ ipfs resolve -r /ipns/vitalik.eth
/ipfs/QmUWvM2pXSMk8kFMDqDnskMnURePtayXUECMYfwUJYgbks
```