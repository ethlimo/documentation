# Gateway Basics

### General Usage

Any properly configured ENS or Gnosis domain is eligible for gateway resolution without any additional configuration. The only requirement is a [properly configured](../beginner/configuring-your-ens-name/updating-your-ens-content-records.md) `contentHash` record. Once a domain has been configured, simply append `.limo` to the end of any `.eth` or `.gno` domain to serve your dWebsite content.

### Caching

By default all content is cached for 5 minutes. This means that if you update the `contenthash` of your ENS domain, it _may_ take _up to_ 5 minutes for those changes to become visible

### CCIP and Offchain Resolution
Both `*.eth.limo` and `*.eth.link` gateways support offchain resolution via [CCIP](https://eips.ethereum.org/EIPS/eip-3668). For more information, please consult the following resources:

* [Unruggable Gateway](https://github.com/unruggable-labs/unruggable-gateways)
* [Cross Chain/Offchain Resolvers](https://docs.ens.domains/resolvers/ccip-read)

### HTTPS Certificates

All subdomain certificates, i.e. non `*.eth.limo` or `*.eth.link` (single left label) are generated on demand, provided that there is a valid resolver set for the subdomain (including CCIP). Users no longer need to request certificates manually in our [Discord server](https://discord.gg/zf8NxW94rB).

### Server Side Headers

If you require custom HTTP response headers to support features such as [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) please contact us in [Discord](https://discord.gg/zf8NxW94rB). We are actively collaborating on an ENSIP specification that will allow ENS domain holders to specify server side headers in a `TXT` record, however this feature is still in heavy development for the time being.

### Supported Storage Protocols

Currently, we support IPFS, IPNS, Arweave, ARNS (Arweave Naming Service), and Swarm.
