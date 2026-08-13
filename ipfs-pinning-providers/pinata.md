---
description: Using Pinata to pin dWebsite content on IPFS
---

# Pinata

[Pinata](https://pinata.cloud/) is one of the longest-running IPFS pinning services, offering a web app for simple uploads, an SDK and API for developers, and dedicated IPFS gateways.

New to pinning? See [What is IPFS Pinning?](../ipfs/what-is-ipfs-pinning.md) for the concepts — Pinata's own [IPFS 101 explainer](https://docs.pinata.cloud/ipfs-101/what-is-ipfs-pinning) covers the same ground: when you upload a file, the node generates a CID and pins it so it survives garbage collection; content stays available as long as at least one node maintains the pin.

### Uploading via the Web App

1. Go to [Pinata.cloud](https://pinata.cloud/) and sign up or log in.
2. Select **Add** and choose your file (for a dWebsite, upload your site folder or `index.html`).
3. Name the file and confirm the upload.
4. The file appears in the files page with its CID, which you can use as your ENS content hash.

### Uploading via the SDK or API

For decentralized applications and automated workflows, Pinata provides an [SDK and API](https://docs.pinata.cloud/):

```typescript
import { PinataSDK } from "pinata";

const pinata = new PinataSDK({
  pinataJwt: "PINATA_JWT",
  pinataGateway: "example-gateway.mypinata.cloud",
});

const file = new File(["Hello IPFS!"], "hello-world.txt", { type: "text/plain" });
const upload = await pinata.upload.file(file);
```

A Pinata CLI and starter templates are also available, authenticated via API keys.

### Pinning an Existing CID

Content that already exists on the IPFS network can be transferred to Pinata using the **Import from IPFS** button in the web app, or programmatically via the Pin by CID endpoint:

```typescript
const pin = await pinata.upload.cid("QmVLwvmGehsrNEvhcCnnsw5RQNseohgEkFNN1848zNzdng")
```

### Using Your CID with ENS

Once your content is pinned, set the CID as the content hash of your ENS name (see [Updating Your ENS Content Records](../beginner/configuring-your-ens-name/updating-your-ens-content-records.md)) and your dWebsite is reachable at `https://yourname.eth.limo`.

### Resources

* [Pinata documentation](https://docs.pinata.cloud/)
* [What is IPFS Pinning? (Pinata IPFS 101)](https://docs.pinata.cloud/ipfs-101/what-is-ipfs-pinning)
