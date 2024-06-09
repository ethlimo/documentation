# Understanding Content Hashes, IPNS, and IPFS for ENS

#### What is a Content Hash?

A content hash is like a digital fingerprint for a specific piece of content. It's a unique identifier created using a cryptographic hash function, ensuring that even the smallest change in the content results in a completely different hash. In the context of ENS (Ethereum Name Service), a content hash links an ENS name (like "myname.eth") to content stored on IPFS (InterPlanetary File System), making decentralized websites and digital content easily accessible.

## Difference Between Content Hash, IPFS, and IPNS

### **IPFS (InterPlanetary File System)**

* **Definition:** IPFS is a decentralized network designed for storing and sharing files. It works by breaking files into smaller chunks, distributing them across many nodes (computers), and addressing them by their content.
* **Content Addressing:** Each file uploaded to IPFS is given a unique Content Identifier (CID) based on its content. This CID is a type of content hash.
* **Usage:** IPFS is used to store and share files in a decentralized manner, avoiding reliance on central servers.

**Example Workflow:**

1. You upload a file called "mydocument.pdf" to IPFS.
2. IPFS breaks the file into chunks, distributes them across the network, and generates a CID like `Qm12345...`.
3. This CID can be used to retrieve the file from the IPFS network.

* **Drawbacks:**
  * If the content changes (even slightly), a new CID is generated.
  * To update the content linked to an ENS name, you need to update the content hash in the ENS records, which involves a new transaction.

**Content Hash**

* **Definition:** A content hash is the unique identifier generated for a file uploaded to IPFS.
* **Use in ENS:** The content hash links an ENS name to the specific CID of the content stored on IPFS.
* **Immutability:** The content hash does not change once generated. If the content is modified, a new content hash is created.

**Example Workflow:**

1. After uploading "mydocument.pdf" to IPFS and getting a CID `Qm12345...`, you link this CID to your ENS name "myname.eth".
2. Now, typing "myname.eth.limo" in a compatible browser retrieves the file "mydocument.pdf" from IPFS.

* **Drawbacks:**
  * If you need to update the content, you must upload the new version to IPFS, get a new CID, and then update the ENS records with this new CID.

### **IPNS (InterPlanetary Name System)**

* **Definition:** IPNS is built on top of IPFS and provides a way to create mutable, human-readable names for content.
* **Mutable Links:** Unlike the immutable CIDs of IPFS, IPNS allows you to create links that can be updated to point to new content.
* **Use Case:** IPNS is ideal for content that changes frequently, such as a blog or an updated website, without changing the address.

**Example Workflow:**

1. You create an IPNS link for your website, initially pointing to CID `Qm12345...`.
2. Later, you update your website, which gets a new CID `Qm67890...`.
3. Instead of changing the ENS records, you update the IPNS link to point to the new CID.
4. The IPNS link remains the same, but it now points to the updated content.

* **Drawbacks:**
  * Requires a dedicated system such as a pinning service or a self hosted solution for users to be able to access the content deployed.&#x20;
