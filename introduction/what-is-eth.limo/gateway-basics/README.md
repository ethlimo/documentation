# Gateway Basics

### Requirements
To use the eth.limo gateway, you need to have an ENS domain with a valid resolver and a supported `contenthash` record. Wildcard resolvers conforming to the [ENSIP-10](https://docs.ens.domains/ensip/10/) standard are supported. Just append `.limo` to the end of any `.eth` or `.gno` domain to serve your dWebsite content.

### How Resolution Works

eth.limo operates a wildcard DNS record (`*.eth.limo`) that captures requests for every ENS domain. On each request the gateway:

1. Extracts the ENS name from the hostname (`ens.eth.limo` → `ens.eth`).
2. Looks up the name's resolver and reads its `contenthash` record from Ethereum (including CCIP-read for off-chain resolvers).
3. Decodes the record and fetches the content from the corresponding storage network — IPFS/IPNS, Arweave/ArNS, Swarm, or directly from chain for data URLs.
4. Serves the content over HTTPS with security headers and per-site origin isolation applied.

### URL Format

| URL | Resolves |
| --- | -------- |
| `https://ens.eth.limo` | The name `ens.eth` |
| `https://app.ens.eth.limo` | The subdomain `app.ens.eth` (any depth of subdomains is supported) |
| `https://12345.gno.limo` | The GNS name `12345.gno` — see [.gno Resolution](../../../gnosis/gateway.md) |

Because each name is served from its own origin (`*.eth.limo` hostname), browser storage, cookies, and permissions are isolated per dWebsite.

### Error Pages

If a name has no `contenthash` record the gateway returns a **404 "ENS Domain Not Configured"** page; if the record exists but can't be resolved it returns **422 "Unsupported Contenthash"**. See [Troubleshooting](../../../troubleshooting.md) for what each error means and how to fix it.

### CCIP and Off-chain Resolution
The `*.eth.limo`, `*.eth.link`, and `*.gno.limo` gateways support off-chain resolution via [CCIP](https://eips.ethereum.org/EIPS/eip-3668). For more information, please consult the following resources:

* [Unruggable Gateway](https://github.com/unruggable-labs/unruggable-gateways)
* [Cross Chain/Offchain Resolvers](https://docs.ens.domains/resolvers/ccip-read)

### Caching

By default all content is cached for 5 minutes. This means that if you update the `contenthash` of your ENS domain, it _may_ take _up to_ 5 minutes for those changes to become visible. We are actively working on a refactor of the caching layer which will eventually deprecate this limitation. If your update still isn't visible after that window, see [Troubleshooting](../../../troubleshooting.md).

### HTTPS Certificates

All subdomain certificates, i.e. non `*.eth.limo` are generated on demand, provided that there is a valid resolver and `contenthash` record set for the subdomain. Users no longer need to request certificates manually in our [Discord server](https://discord.gg/zf8NxW94rB).

### Server Side Headers

If you require custom HTTP response headers to support features such as [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/SharedArrayBuffer) please contact us in [Discord](https://discord.gg/zf8NxW94rB). We are actively collaborating on an ENSIP specification that will allow ENS domain holders to specify server side headers in a `TXT` record, however this feature is still in heavy development for the time being.

### Supported Storage Protocols

Currently, we support IPFS, IPNS, Arweave, ARNS (Arweave Name System), Swarm, [TON Sites](../../../ton/ton-sites.md), and fully on-chain content via [Data URLs & ENS hooks (EIP-8121)](../../../advanced/onchain-data-urls.md).
