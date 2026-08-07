# ENSv2

_Status note (August 2026): reflects the February 2026 announcement ["ENS: Staying on Ethereum"](https://ens.domains/blog/post/ens-staying-on-ethereum). This page tracks what dWebsite operators should know as ENSv2 approaches launch._

ENSv2 is a redesign of the ENS protocol built around a new hierarchical registry architecture. In February 2026, ENS Labs announced that ENSv2 will deploy **exclusively on Ethereum Layer 1** — development of Namechain, the previously planned purpose-built Layer 2, has been halted.

### Why ENS is staying on Ethereum

The original ENSv2 plan assumed L1 costs would keep pricing out everyday name operations. Ethereum's scaling progress overtook that assumption — as the announcement puts it, "Ethereum L1 is scaling, and it's scaling faster than almost anyone predicted two years ago":

* ENS registration gas costs dropped ~99% year-over-year, from roughly $5 to under 5 cents.
* Ethereum's gas limit rose from 30M to 60M in 2025, with 200M targeted for 2026.
* Subsidizing *all* 2025 ENS transactions on L1 would have cost around $10,000 — far less than operating a dedicated L2.

The Namechain engineering work isn't discarded: its designs feed into ENSv2's interoperability with existing L2s and cross-chain user experience.

### What ENSv2 still delivers

* **New registry architecture** with flexible ownership models (hierarchical, per-name registries)
* **Simplified registration** — single-step enrollment, with purchases payable in stablecoins from any chain
* **New apps** — the ENS App and ENS Explorer are in public alpha
* **Multi-chain resolution** for 60+ blockchains, including Bitcoin and Solana

Dropping the cross-chain migration also removes a large source of complexity, improving stability and operational reliability.

### What it means for dWebsites

* **Resolution stays on L1 and stays simple.** Gateways like eth.limo continue reading names directly from Ethereum — no new cross-chain hop is introduced. CCIP-read ([ERC-3668](https://eips.ethereum.org/EIPS/eip-3668)) remains what it is today: the mechanism for names that opt into off-chain or L2 resolvers (see [ENS Subdomains and CCIP](ens-subdomains-ccip.md)). Your `name.eth.limo` URL does not change.
* **Content hash records carry over.** The `contenthash` record type and its protocol encodings ([Record Types](record-types.md)) are unchanged.
* **Cheap management is already here.** The L1 fee collapse that made Namechain unnecessary also means registrations, renewals, and content hash updates now cost cents — and ENS Labs has floated L1 gas subsidies for `.eth` holders once ENSv2 launches.
* **Ecosystem tooling targets ENSv2.** For example, the [ens-hooks](https://github.com/ethlimo/ens-hooks) publishing flow for [on-chain data URLs](onchain-data-urls.md) references ENSv2 names.

### What you need to do

Nothing, currently. Existing names, resolvers, and content hash records continue to work, and eth.limo will track protocol changes on the gateway side. Follow [ENS Labs' blog](https://blog.ens.domains/) for launch timelines.
