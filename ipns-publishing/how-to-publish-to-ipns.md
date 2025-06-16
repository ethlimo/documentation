# IPNS Publishing

### What is IPNS?

InterPlanetary Name System - [IPNS](https://docs.ipfs.tech/concepts/ipns/) is a naming system that allows you to create mutable links to content stored on IPFS. Unlike IPFS, which uses immutable content identifiers (CIDs), IPNS enables you to update the content linked to a specific name using a static reference. Think of IPNS as an anchor that can serve as dynamic pointer to your content, allowing you to modify the underlying data without changing your ENS contentHash or advertising a new CID.

### Why Use IPNS?

IPNS is particularly useful for scenarios where content needs to be updated frequently, such as websites and blogs. By using IPNS, you can ensure that your users always access the latest version of your content without needing to modify your ENS contentHash record. For example, the contentHash record for `domain.eth` can be set to `ipns://k51...` (truncated). `ipns://k51...` is a pointer to `ipfs://bafy...` (truncated), which can then be updated to reference a new IPFS CID whenever the content changes. These IPNS record updates live off-chain and are managed by the underlying IPFS network protocol.

### Hosted IPNS Service Providers

While there are many excellent hosted services available, we typically recommend either [Fleek](https://fleek.xyz/docs/platform/hosting/) or [Filebase](https://filebase.com/) for IPNS publishing.

### Self-Hosting IPNS

Production ready configurations for self-hosting are outside of the scope of this guide, but if you want to run your own IPFS node for publishing IPNS records, you can follow these steps:

1. Download and install [Kubo](https://github.com/ipfs/kubo/).
2. Initialize your IPFS node:
   ```bash
   ipfs init --profile server
   ```
3. Start your IPFS node:
   ```bash
   ipfs daemon
   ```
4. Generate a CID for your content:
    ```bash
    ipfs add --cid-version 1 <file|directory>
    ```
5. Pin the CID produced in the previous step:
   ```bash
   ipfs pin add <CID>
   ```
6. Associate the CID with an IPNS public key:
    ```bash
    ipfs name publish <CID>
    ```

Step 6 will return an IPNS public key record type that looks like `k51...` (truncated). You can then use this IPNS public key as your ENS domain's contentHash, which will associate it with the CID produced in step 4. You can repeate these steps whenever you need to update the content, and the IPNS record will point to the latest CID.

