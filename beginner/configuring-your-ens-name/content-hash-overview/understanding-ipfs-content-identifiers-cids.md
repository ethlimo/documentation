# Understanding IPFS Content Identifiers (CIDs)

#### What is a CID?

The string of letters and numbers you see in an IPFS link is its "Content Identifier" or CID. A CID is a unique label used to point to a specific piece of content within the IPFS network. This unique identifier ensures that content is always retrievable in its original form.

#### Types of CIDs

IPFS uses two types of CIDs:

**CID Version 0**

When IPFS was first designed, it used base 58-encoded multihashes as the content identifiers, which looked like this:

```
QmQbeKEi4XJ9yLL2nyFKxpuPyrfqmHBGtRj3EhWcSvQaSD
```

Many applications and services still use CIDv0 by default, but this format is nearing replacement.

**CID Version 1**

The IPFS project is transitioning to CIDv1 as the new default format. CIDv1 is a more flexible and future-proof approach. An example of a CIDv1 looks like this:

```
bafybeibbr2r6b3hd3p5jggobmgacm7f4523ijyza364szdeyrw3b4uy7ei
```

CIDv1 includes several leading identifiers that provide additional information:

* **Multibase Prefix:** Specifies the encoding used for the rest of the CID.
* **CID Version Identifier:** Indicates which version of CID is being used.
* **Multicodec Identifier:** Specifies the format of the target content, helping both people and software understand how to interpret the content once fetched.

These leading identifiers ensure forward-compatibility, allowing different formats to be used in future versions of CID.

#### Troubleshooting CIDs

**Different CID Shows After Setting**

When you set a CIDv0 on your ENS name, it will be converted to a CIDv1. For instance, if you enter a CIDv0 like this:

```
QmQbeKEi4XJ9yLL2nyFKxpuPyrfqmHBGtRj3EhWcSvQaSD
```

Once it's set on your ENS name, it will be converted to look like this:

```
bafybeibbr2r6b3hd3p5jggobmgacm7f4523ijyza364szdeyrw3b4uy7ei
```

**Example Scenario**

If you edit the Content Hash record for your ENS name and set it to:

```
ipfs://QmQbeKEi4XJ9yLL2nyFKxpuPyrfqmHBGtRj3EhWcSvQaSD
```

You will see your record reformatted to the CIDv1 version of the IPFS hash:

```
ipfs://bafybeibbr2r6b3hd3p5jggobmgacm7f4523ijyza364szdeyrw3b4uy7ei
```

By understanding these details about CIDs, you can more effectively manage and troubleshoot your ENS-linked content. The eth.limo service gateway makes this process straightforward, ensuring your content is always accessible and correctly formatted.&#x20;
