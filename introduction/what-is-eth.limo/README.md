---
description: The last mile of Defi
---

# What is eth.limo?

A privacy preserving ENS gateway for resolving Ethereum Name Service (ENS) records and associated IPFS/IPNS/Skynet content (Web 3.0). LIMO allows users and dApp developers to effortlessly access and host static sites built with a combination of IPFS/IPNS/Arweave and ENS.

LIMO is intended to be operated as a reliable and secure public good for all users. Documentation can be found [here](https://github.com/ethlimo/documentation). The v1 infrastructure codebase is also [available](https://github.com/eth-limo/eth.limo).

### Why do we need ETH.LIMO? <a href="#why-limo" id="why-limo"></a>

Core internet infrastructure is becoming increasingly centralized and as such, several existential threats have emerged that undermine the principles of an decentralized, open and free internet:

* Single points of failure (Cloudflare, CDNs, etc)
* Eth.link is unreliable
* All ENS gateway infrastructure is currently operated by a single entity (Cloudflare)
* Censorship
* Lack of non-commercial infrastructure
* Opacity of terms of service agreements and moderation policies
* Proliferation of proprietary browser extensions which require centralized services (Unstoppable domains, etc…)
* The erosion of the “commons” and “public space”
* Web 3.0 adoption is still nascent
* Most browsers cannot natively access ENS/IPFS/IPNS/Arweave

For end users, this means a limited selection of poor quality services that increasingly resemble a cable TV package. Accessing the dWeb can be a frustrating experience if eth.link is unavailable.

For dApp developers, this leads to a limited selection of platforms and services in addition to lost revenue. As an alternative to eth.link for users and dApp developers, any existing eth.link site can be accessed with eth.limo in the same manner.

The LIMO project brings additional resiliency to dApps by providing an alternative means of access as a public good. Application owners can directly participate in the roadmap of LIMO, giving key stakeholders influence over the services they depend upon. As the decentralized web continues to grow, it is imperative that community projects facilitate the transition from Web 2.0 to Web 3.0.

### How does ETH.LIMO work? <a href="#how-limo-works" id="how-limo-works"></a>

LIMO operates as a reverse proxy for ENS names and IPFS content.

Much like eth.link, LIMO uses a wildcard DNS record \*.eth.limo to dynamically capture requests for all ENS domains. The LIMO service automatically resolves the IPFS/IPNS/Arweave contenthash of the requested ENS record and returns the corresponding static content over HTTPS. Since native IPFS/IPNS/Arweave resolution capabilities are missing from the majority of browsers, ETH.LIMO represents a bridge from the “normal” internet to the decentralized one.

Zero-configuration is required in order to access or host a site or dApp on ETH.LIMO, making ETH.LIMO an excellent no-cost solution for providing Web 2.0 access to your project.

### Security and Privacy <a href="#security-privacy" id="security-privacy"></a>

The LIMO project enforces strong origin isolation policies for all resources by default. Projects accessed through ETH.LIMO automatically receive security hardened browser headers and all client data is encrypted with TLSv1.3 (default) or TLSv1.2 using the latest ciphersuites. All log data is fully anonymized, which prevents the ability to uniquely identify any single user.

### Additional Resources <a href="#further-reading" id="further-reading"></a>

| [The last mile of DeFi](https://medium.com/@ethdotlimo/what-is-eth-limo-faf18dcaa36d) | [Giveth](https://giveth.io/project/ethlimo)         |
| ------------------------------------------------------------------------------------- | --------------------------------------------------- |
| [Substack](https://ethlimo.substack.com/)                                             | [Twitter](http://www.twitter.com/eth\_limo)         |
| [Email Us](mailto:hello@eth.limo)                                                     | [Report Abuse](https://forms.gle/n85iLgSuim9mmP6EA) |
| [Privacy Policy](https://eth.limo/privacy.html)                                       | [Terms of Service](https://eth.limo/tos.html)       |
| [DMCA](https://eth.limo/dmca.html)                                                    | [Calendly](https://calendly.com/ethdotlimo/)        |
