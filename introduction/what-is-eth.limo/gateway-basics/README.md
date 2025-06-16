# Gateway Basics

### Requirements
To use the eth.limo gateway, you need to have an ENS domain with a valid resolver and a supported `contenthash` record. Off-chain record retrieval is supported via CCIP-read [ERC-3668](https://eips.ethereum.org/EIPS/eip-3668). Wildcard resolvers conforming to the [ENSIP-10](https://docs.ens.domains/ensip/10/) standard are also supported.

### Caching

By default all content is cached for 5 minutes. This means that if you update the `contenthash` of your ENS domain, it _may_ take _up to_ 5 minutes for those changes to become visible. We are actively working on a refactor of the caching layer which will eventually deprecate this limitation.

### HTTPS Certificates

All subdomain certificates, i.e. non `*.eth.limo` are generated on demand, provided that there is a valid resolver and `contenthash` record set for the subdomain. Users no longer need to request certificates manually in our [Discord server](https://discord.gg/zf8NxW94rB).

### Server Side Headers

If you require custom HTTP response headers to support features such as [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/SharedArrayBuffer) please contact us in [Discord](https://discord.gg/zf8NxW94rB). We are actively collaborating on an ENSIP specification that will allow ENS domain holders to specify server side headers in a `TXT` record, however this feature is still in heavy development for the time being.

### Supported Storage Protocols

Currently, we support IPFS, IPNS, Arweave, ARNS (Arweave Name System), and Swarm.
