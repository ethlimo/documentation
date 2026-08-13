---
description: How IPFS HTTP gateways work and the different types available
---

# IPFS Gateways

An IPFS gateway is an HTTP bridge to the IPFS network: it lets any web browser or HTTP client fetch content-addressed data (CIDs) without running IPFS itself. eth.limo is itself a gateway that combines ENS resolution with IPFS (and other protocol) retrieval.

Gateways expose content through two common URL styles:

* **Path based**: `https://<gateway>/ipfs/<CID>`
* **Subdomain based**: `https://<CID>.ipfs.<gateway>` (provides per-CID origin isolation in the browser)

Gateways can serve the CID of a folder instead of just a single file — returning a directory listing — and can serve entire static websites from a folder containing HTML and assets.

### How a Gateway Retrieves Content

When a request for a CID reaches an IPFS HTTP gateway:

1. The gateway checks whether the CID is already cached locally (in the HTTP cache or the gateway node's cache).
2. If not cached, the CID must be retrieved from the IPFS network.
3. The gateway's IPFS peer first asks its directly connected peers if any of them host the requested CID, then queries the DHT (and delegated routing endpoints) to find peers that are hosting or pinning it.
4. The gateway connects to one of those peers, fetches the content, verifies it against the CID, and relays the response to the requesting client.

### Public vs Private Gateways

**Public gateways** allow anyone to use HTTP to retrieve CIDs from the IPFS network. They are typically rate-limited and may restrict certain content types to prevent abuse (see our provider pages for the [Filebase](../ipfs-pinning-providers/using-the-filebase-public-ipfs-gateway.md) and [4Everland](../ipfs-pinning-providers/using-the-4everland-public-ipfs-gateway.md) public gateways).

**Private gateways** come in two forms:

* **Dedicated gateways** are offered by IPFS pinning services and give you your own gateway endpoint for the CIDs pinned with that service. Dedicated gateways are typically not subject to the rate limits imposed on public gateways.
* **Self-hosted gateways** are IPFS nodes you run yourself — locally or in the cloud — configured to act as a gateway. They also avoid rate limits, but require you to operate the node. Many users choose a pinning service with dedicated gateways to avoid hosting IPFS nodes themselves. If you'd rather self-host, see [Running a Local Gateway](../local-gateway/running-a-local-gateway.md) for the complete eth.limo stack, or [Running Your Own IPFS Node](../intermediate/running-your-own-ipfs-node.md) for a plain Kubo node.
