# ENSv2 and Namechain

_Status note (August 2026): ENSv2 is under active development by ENS Labs. Details below may change; this page tracks what dWebsite operators should know._

ENSv2 is a redesign of the ENS protocol that migrates `.eth` name management from Ethereum mainnet to **Namechain**, a purpose-built Layer 2, alongside a hierarchical registry architecture that gives each name its own registry contract.

### What it means for dWebsites

* **Resolution remains compatible.** Gateways like eth.limo resolve names through the existing resolution path, with cross-chain reads handled by [CCIP-read (ERC-3668)](https://eips.ethereum.org/EIPS/eip-3668) — the same mechanism already used for off-chain and L2 resolvers today (see [ENS Subdomains and CCIP](ens-subdomains-ccip.md)). Your `name.eth.limo` URL does not change.
* **Content hash records carry over.** The `contenthash` record type and its protocol encodings ([Record Types](record-types.md)) are unchanged by the migration.
* **Cheaper management.** Registrations, renewals, and record updates on an L2 mean significantly lower gas costs for maintaining a dWebsite.
* **New capabilities.** Tooling in the ecosystem is already building against ENSv2 — for example, the [ens-hooks](https://github.com/ethlimo/ens-hooks) publishing flow for [on-chain data URLs](onchain-data-urls.md) references ENSv2 names.

### What you need to do

Nothing, currently. Existing names, resolvers, and content hash records continue to work throughout the transition, and eth.limo will track protocol changes on the gateway side. Follow [ENS Labs' announcements](https://blog.ens.domains/) for migration timelines.
