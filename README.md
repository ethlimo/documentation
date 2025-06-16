# What is eth.limo?

eth.limo is a public goods project that provides a free, open-source gateway to decentralized web content built with the Ethereum Name Service (ENS) and various decentralized storage protocols. It simplifies access to decentralized web content by resolving ENS names and content identifiers, allowing users to interact with the decentralized web without needing to run their own clients or manage complex configurations. More information can be found in our [introduction](./introduction/what-is-eth.limo/README.md) section.

### TL;DR

Any [properly configured](./beginner/configuring-your-ens-name/updating-your-ens-content-records.md) ENS domain can be accessed through the eth.limo gateway. For example, `foo.eth.limo` will resolve to the content associated with the ENS name `foo.eth`, whether it's hosted on IPFS, Arweave, Swarm, or any other supported decentralized storage protocol. No configuration is required to use eth.limo, either as a user or a developer; simply add `.limo` to the end of your ENS domain name in any browser and you're on your way!

Storage protocol support includes: [IPFS](https://ipfs.tech/), [IPNS](https://docs.ipfs.tech/concepts/ipns/), [Arweave](https://www.arweave.org/), [Arweave Name System](https://ar.io/arns), and [Swarm](https://www.ethswarm.org/).

### Want to run your own local gateway?

Check out our API [repo](https://github.com/ethlimo/dweb-proxy-api) for instructions on how to set up your own local ENS gateway.

# What is ENS?

### 🔐 Security Simplified

**Transforming Digital Identity Since 2017**

The [Ethereum Name Service (ENS)](https://ens.domains) revolutionizes the way we interact with blockchain technology, making Ethereum addresses user-friendly with '.eth' domain names. This innovation extends beyond Ethereum, with compatibility for a wide range of identifiers including Bitcoin, Litecoin, IPFS hashes, and much more.

**Example: Ethereum Address Resolution**

* Before ENS: `0x1234...abcd`
* After ENS: `john.eth`

**Example: Cross-Chain Address Resolution**

* For Bitcoin: `john.eth` resolves to `1A1zP1eP...8G8ijZ`
* For Litecoin: `john.eth` resolves to `LZyx...G6RG`

**Example: IPFS Content Resolution**

Using ENS, `john.eth` can link to an IPFS hash, such as `QmTz...XiT`, pointing directly to decentralized web content. Through `eth.limo` as a proxy, accessing this content is simplified: `john.eth.limo` in your browser takes you straight to the IPFS content.

### 🧭 Guiding Navigation

**Establish Your Digital Footprint**

ENS empowers users to anchor their digital identity in the blockchain universe. Registering an ENS domain simplifies digital transactions, smart contract engagements, and strengthens your presence on the decentralized web. Claim your unique .eth domain to navigate the Ethereum blockchain effortlessly.

Start your journey at [ENS Domains](https://app.ens.domains/).

### 🌍 Web Unification

**Beyond Naming: A Decentralized Web Framework**

ENS is more than a naming system; it's a framework for the decentralized web. With its ability to point to IPFS hashes, ENS is not just for sending and receiving cryptocurrency but also for accessing decentralized websites and content stored on IPFS, facilitating a more user-friendly experience on the decentralized web.

Top-Level Domains (TLDs), like `.eth` and `.test`, are owned by smart contracts called [registrars](https://docs.ens.domains/registry/eth), which specify rules governing the allocation of their names. Enabling seamless interoperability with the DNS (Domain Name System).

### ETH Registrar <a href="#eth-registrar" id="eth-registrar"></a>

The [ETH Registrar](https://docs.ens.domains/registry/eth) is the registrar for the `.eth` TLD, it allows for trustless decentralized names to be issued as tokens on the Ethereum Blockchain. Registration is done through smart-contracts, and name ownership is secured by the Ethereum blockchain.

### DNS + ENS <a href="#dns-ens" id="dns-ens"></a>

ENS has similar goals to DNS, the existing Internet's Domain Name Service, and aims to extend its capability. ENS also supports importing DNS names through the use of DNSSEC. Allowing you to take your `.com`, `.xyz`, or `.art` (and more) into the ENS ecosystem. \
\
Read more about DNSSEC names [on this page](https://docs.ens.domains/learn/dns).

### Subnames <a href="#subnames" id="subnames"></a>

Because of the hierarchal nature of ENS, anyone who owns a domain at any level can take control of resolution. Users can create subdomains manually, or take matters into their own hands and write their own resolution logic.

For instance, if Alice owns 'alice.eth', she can create 'pay.alice.eth' and configure it as she wishes. Or, use a [Custom Resolver](https://docs.ens.domains/resolvers/quickstart), and programmatically issue subdomains, for example in an App, Community, or DAO.

### ENS Manager App <a href="#ens-manager-app" id="ens-manager-app"></a>

You can try ENS out for yourself now by using the [ENS Manager App](https://ens.app/), or by using any of the many ENS enabled applications on the ENS[ homepage](https://ens.domains/).
