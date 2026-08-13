---
description: Why pinning is required to keep IPFS content available
---

# What is IPFS Pinning?

IPFS pinning refers to the process of specifying data to be retained and persist on one or more IPFS nodes. Pinning assures that data is accessible indefinitely, and will not be removed during the IPFS garbage collection process.

### Understanding the Garbage Collection Process

When files and data are stored on the IPFS network, nodes on the network cache the files that they download and keep those files available for other nodes on the network. Since storage on these nodes is finite, the cache for each node must be cleared periodically to make room for new files to be cached and made available. The process of clearing the cache for IPFS nodes is referred to as the IPFS garbage collection process.

Garbage collection is an automatic process that is used to manage resources, such as IPFS node disk space. The process is designed to remove cached data that it thinks is no longer needed. If your IPFS CID refers to a file that is vital to your workflow — such as the current version of your dWebsite — having that file removed can be detrimental.

### Pinning

To protect data from the garbage collection process, data must be pinned on the IPFS network. This ensures that data is retained indefinitely and is always accessible. Pinning is useful for a variety of workflows, such as accessing data files from around the world without managing sharing permissions. All data uploaded to IPFS is public by default since all you need to access it is the file's CID — there are no permissions, user accounts, or other security settings tied to IPFS CIDs.

For dWebsites this matters directly: if the CID referenced by your ENS content hash is garbage-collected everywhere, your site stops resolving. One of the most visible examples of unpinned content disappearing is NFT collections — if image and metadata files are not pinned and get removed by garbage collection, the NFT effectively ceases to exist ("NFT rug pull").

### Where To Pin

You have two options for keeping content pinned:

1. **Pin on your own node** — configure pinning on a locally hosted IPFS node. See [Running Your Own IPFS Node and IPNS Publishing](../intermediate/running-your-own-ipfs-node.md).
2. **Use a pinning service** — services run many IPFS nodes and pin your data for you, providing external, long-term persistence without maintaining infrastructure. See our provider guides for [Filebase](../ipfs-pinning-providers/filebase.md), [4Everland](../ipfs-pinning-providers/4everland.md), [Pinata](../ipfs-pinning-providers/pinata.md), and [PinMe](../ipfs-pinning-providers/pinme.md), or the beginner-level [Hosting using a pinning service](../beginner/how-to-use-ipfs-ipns/uploading-to-ipfs/hosting-using-a-pinning-service.md).

For more background, see the IPFS project's [Persistence documentation](https://docs.ipfs.tech/concepts/persistence/).
