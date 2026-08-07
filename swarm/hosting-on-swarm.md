---
description: Publishing your dWebsite on Ethereum Swarm
---

# Hosting on Swarm

[Swarm](https://www.ethswarm.org/) is Ethereum's native decentralized storage and communication network. Content uploaded to Swarm is addressed by a 64-character hex **Swarm reference** (a `bzz` hash), which you can set as your ENS content hash — eth.limo resolves and serves it just like IPFS or Arweave content.

There are two ways to publish: a managed web interface (no infrastructure required), or running your own Bee node.

### Option 1: Beeport (Managed)

[Beeport](https://beeport.bzz.link/) is a web interface — "the web2 rails for Swarm" — that lets you upload and share files on Swarm without running a node.

1. Open [beeport.bzz.link](https://beeport.bzz.link/) in your browser.
2. Purchase storage (Swarm storage is paid for with **storage stamps**) and select a storage duration.
3. Upload your file or site.
4. Copy the resulting Swarm reference for use in your ENS content hash (see below).

{% hint style="info" %}
Beeport is currently in beta. For critical or large-scale deployments, the Beeport team itself recommends running your own Bee node.
{% endhint %}

### Option 2: Running a Bee Node (Self-Hosted)

[Bee](https://github.com/ethersphere/bee) is the Swarm Foundation's official node client. Follow the official [getting started guide](https://docs.ethswarm.org/docs/bee/installation/getting-started/) for the full walkthrough; the essentials:

**Node types** — pick based on your use case:

| Node type | Use case |
| --------- | -------- |
| Ultra-light | Small free-tier downloads only |
| Light | Uploading and downloading without joining the incentive system |
| Full | Everything, including earning xBZZ and supporting the network |

For publishing a dWebsite, a **light node** is usually sufficient.

**Requirements:**

* A Gnosis Chain RPC endpoint (postage stamps and transactions settle on Gnosis Chain) — self-hosted or a public/paid endpoint
* Light/ultra-light nodes run on modest consumer hardware; full nodes want a recent dual-core 2 GHz CPU, 8 GB RAM, 30 GB SSD, and a stable connection
* Home setups may need NAT/port forwarding; VPS installs usually don't

**Installation** — several supported methods, per the official guide:

* [Swarm Desktop](https://www.ethswarm.org/build/desktop) (GUI, beginner-friendly)
* Shell script install
* Docker (the eth.limo [local gateway](../local-gateway/running-a-local-gateway.md) runs Bee this way)
* Package managers (APT, RPM, Homebrew)
* Build from source

**Publishing flow:**

1. Start your Bee node and fund it for postage (light/full nodes).
2. Purchase a **postage stamp batch** — this pays for your content to be stored on the network for a chosen duration and depth.
3. Upload your site directory via the Bee API (default `localhost:1633`) or [swarm-cli](https://github.com/ethersphere/swarm-cli), referencing your postage batch.
4. The upload returns a Swarm reference (64-hex string).

Consult the official docs for current upload commands and postage stamp guidance: [docs.ethswarm.org](https://docs.ethswarm.org/).

### Using Your Swarm Reference with ENS

Set your ENS content hash to the Swarm reference with the `bzz://` prefix (see [Updating Your ENS Content Records](../beginner/configuring-your-ens-name/updating-your-ens-content-records.md)):

```
bzz://<64-character-swarm-reference>
```

Your dWebsite is then reachable at `https://yourname.eth.limo`, or through your own [local gateway](../local-gateway/running-a-local-gateway.md) at `https://yourname.eth.localhost` (the local stack includes a Bee light client and also serves raw references at `https://{reference}.swarm.localhost`).

### Keeping Content Alive

Unlike IPFS pinning (indefinite until unpinned), Swarm storage is leased: content persists as long as its postage stamp batch has value and hasn't expired. Monitor and top up your batches, or your site will eventually be garbage-collected by the network.

### Resources

* [Official Bee getting started guide](https://docs.ethswarm.org/docs/bee/installation/getting-started/)
* [Beeport](https://beeport.bzz.link/)
* [Swarm documentation](https://docs.ethswarm.org/)
