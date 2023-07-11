# eth.limo Documentation
[eth.limo](https://eth.limo) (LIMO) is a free, [private](https://eth.limo/privacy.html), and secure public good for accessing Ethereum Name Service (ENS) domains over HTTPS or DNS over HTTPS (DoH). LIMO currently supports IPFS, IPNS, and Arweave `contenthash` [values](https://eips.ethereum.org/EIPS/eip-1577).

In the event that a given ENS domain does _not_ have a `contenthash` defined, we will serve a dynamically generated [Nimi](https://nimi.eth.limo) profile. Our integration with Nimi provides any ENS domain (without a `contenthash`) with a standardized social landing page. Please see [nick.eth](https://nick.eth.limo) as an example.

### Table of contents
* [Gateway Basics](./gateway/http.md)
* [DNS over HTTPS (DoH)](./dns-over-https/doh.md)
* [Using eth.limo with IPFS (Kubo)](./ipfs/config.md)
* [.ART resolution](./art/config.md)

### Support

Please use the `#community-support` channel in our [Discord server](https://discord.gg/zf8NxW94rB) for any questions. Feature requests should be submitted to the `community-forum` channel.

### ENS Grant Updates

You can track the latest eth.limo ENS grant updates [here](https://discuss.ens.domains/t/term-3-grants-summary/16549/2).
