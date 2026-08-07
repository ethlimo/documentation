---
description: Resolving TON Sites through ENS and the eth.limo gateway stack
---

# TON Sites

[TON](https://ton.org/) (The Open Network) is more than a blockchain — it combines a multi-blockchain platform with its own networking stack: **TON DNS** (human-readable `.ton` names), **TON Storage** (decentralized file sharing), **TON Proxy** (encrypted ADNL tunnels), and **TON Sites**.

A **TON Site** is a web service reached over the TON network's ADNL and RLDP protocols instead of ordinary IP infrastructure. Each site is identified by a 256-bit **ADNL address** derived from a public key, making it location-independent. A request flows like this:

1. A proxy client receives a normal HTTP request from your browser.
2. The proxy resolves the target — via TON DNS for `.ton` domains, or directly from an ADNL address.
3. The HTTP request is encapsulated in RLDP datagrams and sent over ADNL to the site's address.
4. The TON Site responds with a standard HTTP response.

See the official docs for the full picture: [TON web3 overview](https://docs.ton.org/foundations/web3/overview) and [TON Sites](https://docs.ton.org/foundations/web3/ton-sites).

## TON Sites and ENS

The eth.limo gateway stack can resolve ENS names whose content hash points at a TON Site. Set your ENS content hash to the site's ADNL address:

```
adnl://<adnl-address>
```

When TON support is enabled, the gateway resolves the record and proxies the request to the TON network through an RLDP-HTTP gateway, exactly as it proxies IPFS or Swarm content. The DoH resolver returns such records as `dnslink=adnl://<address>`.

{% hint style="warning" %}
TON Site resolution is **experimental**. In the [local gateway](../local-gateway/running-a-local-gateway.md) stack the resolver ships with TON support enabled (`TON_ENABLED=true`), but the RLDP-HTTP proxy container (`tonutils-proxy`) is not enabled by default — you must supply and uncomment a proxy image in `docker-compose.yml`. Once configured, TON Sites are served locally at `https://{adnl-address}.adnl.localhost`.
{% endhint %}

## Hosting Your Own TON Site

To publish a TON Site you run a reverse proxy that bridges your ordinary web server onto the TON network. Two implementations exist:

**rldp-http-proxy** (official, part of the TON toolchain) — requires manual ADNL key generation:

```bash
mkdir keyring
utils/generate-random-id -m adnlid
mv <hex-address>* keyring/
rldp-http-proxy -a <ip>:3333 -L '*' -C global.config.json -A <adnl-address> -d -l tonsite.log
```

**tonutils-reverse-proxy** (Go implementation) — automates key generation and domain linking:

```bash
wget https://github.com/tonutils/reverse-proxy/releases/latest/download/tonutils-reverse-proxy-linux-amd64
chmod +x tonutils-reverse-proxy-linux-amd64
./tonutils-reverse-proxy --domain <domain>.ton
```

On first run it generates a persistent key pair and shows a QR code to confirm domain ownership with your TON wallet.

## Addressing

TON Sites support two access paths:

* **TON DNS** — a `dns_adnl_address` record maps a `.ton` domain to the site's ADNL address. Assignment happens in TON DNS management, confirmed by the domain owner's wallet.
* **Direct ADNL access** — sites are reachable by their raw ADNL address without any DNS record, which is exactly what the `adnl://` ENS content hash uses.

## Resources

* [TON web3 overview](https://docs.ton.org/foundations/web3/overview)
* [TON Sites — direct access via ADNL](https://docs.ton.org/foundations/web3/ton-sites#direct-access-via-adnl-address)
* [tonutils reverse proxy](https://github.com/tonutils/reverse-proxy)
