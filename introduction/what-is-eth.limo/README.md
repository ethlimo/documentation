---
description: The last mile of Defi
---

# What is eth.limo?

A privacy preserving ENS gateway for resolving Ethereum Name Service (ENS) records and associated IPFS/IPNS/Skynet content (Web 3.0). LIMO allows users and dApp developers to effortlessly access and host static sites built with a combination of IPFS/IPNS/Arweave and ENS. Just add `.limo` to the end of your ENS name and you are ready to go!

LIMO is intended to be operated as a reliable and secure public good for all users. Documentation can be found [here](https://github.com/ethlimo/documentation). The current API codebase is available [here](https://github.com/ethlimo/dweb-proxy-api).

### How does ETH.LIMO work? <a href="#how-limo-works" id="how-limo-works"></a>

LIMO operates as a reverse proxy for ENS names and IPFS content.

eth.limo uses a wildcard DNS record \*.eth.limo to dynamically capture requests for all ENS domains. The LIMO service automatically resolves the IPFS/IPNS/Arweave/Swarm contenthash of the requested ENS record and returns the corresponding static content over HTTPS. Since native IPFS/IPNS/Arweave/Swarm resolution capabilities are missing from the majority of browsers, ETH.LIMO represents a bridge from the “normal” internet to the decentralized one.

Zero-configuration is required in order to access or host a site or dApp on ETH.LIMO, making ETH.LIMO an excellent no-cost solution for providing Web 2.0 access to your project.

### Security and Privacy <a href="#security-privacy" id="security-privacy"></a>

The LIMO project enforces strong origin isolation policies for all resources by default. Projects accessed through ETH.LIMO automatically receive security hardened browser headers and all client data is encrypted with TLSv1.3 (default) or TLSv1.2 using the latest ciphersuites. All log data is fully anonymized, which prevents the ability to uniquely identify any single user.

### Additional Resources <a href="#further-reading" id="further-reading"></a>

| [Service Overview](https://ethlimo.substack.com/p/ethlimo-everything-youve-wanted-to) | [Giveth](https://giveth.io/project/ethlimo)         |
| ------------------------------------------------------------------------------------- | --------------------------------------------------- |
| [Substack](https://ethlimo.substack.com/)                                             | [Twitter](http://www.twitter.com/eth\_limo)         |
| [Email Us](mailto:hello@eth.limo)                                                     | [Report Abuse](https://forms.gle/n85iLgSuim9mmP6EA) |
| [Privacy Policy](https://eth.limo/privacy.html)                                       | [Terms of Service](https://eth.limo/tos.html)       |
| [DMCA](https://eth.limo/dmca.html)                                                    | [Calendly](https://calendly.com/ethdotlimo/)        |
