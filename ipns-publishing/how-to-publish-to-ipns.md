# IPNS Publishing

### What is IPNS?

InterPlanetary Name System - [IPNS](https://docs.ipfs.tech/concepts/ipns/) is a naming system that allows you to create mutable links to content stored on IPFS. Unlike IPFS, which uses immutable content identifiers (CIDs), IPNS enables you to update the content linked to a specific name using a static reference. Think of IPNS as an anchor that can serve as dynamic pointer to your content, allowing you to modify the underlying data without changing your ENS contentHash or advertising a new CID.

### Why Use IPNS?

IPNS is particularly useful for scenarios where content needs to be updated frequently, such as websites and blogs. By using IPNS, you can ensure that your users always access the latest version of your content without needing to modify your ENS contentHash record. For example, the contentHash record for `domain.eth` can be set to `ipns://k51...` (truncated). `ipns://k51...` is a pointer to `ipfs://bafy...` (truncated), which can then be updated to reference a new IPFS CID whenever the content changes. These IPNS record updates live off-chain and are managed by the underlying IPFS network protocol.

### Hosted IPNS Service Providers

While there are many excellent hosted services available, we typically recommend either [Fleek](https://fleek.xyz/docs/platform/hosting/) or [Filebase](https://filebase.com/) for IPNS publishing.

### Self-Hosting IPNS

If you want to run your own IPFS node for publishing IPNS records, follow [Running Your Own IPFS Node and IPNS Publishing](../intermediate/running-your-own-ipfs-node.md) — it covers the full workflow (installing Kubo, adding and pinning content, `ipfs name publish`) along with republishing strategies and TTL management.

In short: publishing returns an IPNS public key that looks like `k51...` (truncated), which you set as your ENS domain's contentHash. Whenever your content changes, you republish the new CID under the same key — the IPNS record points to the latest CID and your ENS record never needs to change.

