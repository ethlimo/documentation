# Using the Filebase Public IPFS Gateway

#### The Filebase public IPFS gateway is: <a href="#the-filebase-public-ipfs-gateway-is" id="the-filebase-public-ipfs-gateway-is"></a>

`https://ipfs.filebase.io/ipfs/<CID>`

The Filebase Public IPFS Gateway has an effective rate limit of 200 RPM (requests per minute).

The Filebase Public IPFS Gateway only serves content stored on Filebase.

Gateways can serve the CID of a folder instead of just a single file. In this scenario, it will return a file directory tree that contains the files located in the folder.

For example, the following IPFS gateway URL leads to a folder directory of files:

`https://ipfs.filebase.io/ipfs/QmWTqpfKyPJcGuWWg73beJJiL6FrCB5yX8qfcCF4bHvane`

Gateways can also be used to serve static websites. The following URL leads to a static webpage, hosted on IPFS using a folder containing a static HTML file and image files:

`https://ipfs.filebase.io/ipfs/QmYRpH3myNKG2XeaBmdidec3R5HcF9PYBHUVHfks5ysTpq/`

### Public IPFS Gateways vs Private IPFS Gateways <a href="#public-ipfs-gateways-vs-private-ipfs-gateways" id="public-ipfs-gateways-vs-private-ipfs-gateways"></a>

Public gateways allow anyone to use HTTP to retrieve CIDs from the IPFS network. Filebase offers a public IPFS gateway with the following address:

`https://ipfs.filebase.io/ipfs/<CID>/`

There are two forms of private IPFS gateways: dedicated and self-hosted.

Dedicated gateways are offered by IPFS pinning services that allow you to access the CIDs pinned on IPFS through the pinning service through your own private dedicated gateway. Dedicated gateways are typically not limited to traditional rate limits that public gateways are.

Self-hosted gateways refer to an IPFS node that you host yourself, either in the cloud or on your local machine, that is configured to act as an IPFS gateway. Self-hosted gateways are not limited to rate limits either, but require IPFS to be running and managed by you for functionality. Many users choose an IPFS pinning service that offers dedicated gateways so they can avoid hosting and maintaining IPFS nodes themselves.

## IPFS HTTP Gateway Process <a href="#ipfs-http-gateway-process" id="ipfs-http-gateway-process"></a>

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FP2aBEitxRHO7ZvuFwJvk%252Fimage.png%3Falt%3Dmedia%26token%3Da2788733-ed8e-4f1b-a754-4ec881b84df3&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=0dde8f4ba7e01cfd873b739c39f4db45b739f0f12842486a93c4099f08f61714" alt=""><figcaption></figcaption></figure>

When a request for a CID is initiated using an IPFS HTTP gateway, the following steps occur:

1. The gateway checks if the CID has been cached locally before it attempts to retrieve it from the IPFS network. The cache can either be the local HTTP cache, or the cache of the IPFS gateway node.
2. If the CID hasn’t been cached, the CID will need to be retrieved from the IPFS network.
3. The IPFS peer will first ask its direct peers if any of them are hosting the requested CID, then it will query the DHT to find peer IDs and network addresses of peers that are currently hosting or pinning the requested CID.
4. The IPFS gateway node will connect to one of the peers with the CID, fetch the CID’s content, then relay the response to the client that requested the CID.

## Creating a Dedicated Gateway <a href="#creating-a-dedicated-gateway" id="creating-a-dedicated-gateway"></a>

Navigate to the [Gateways page](https://console.filebase.com/gateways) on the Filebase web console.

Select the ‘Create Gateway’ button in the upper right corner.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252Fny1IMROnKh7pKX7TePy7%252Fimage.png%3Falt%3Dmedia%26token%3Dc642cc5a-b624-4969-b240-c70c21e30edd&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=5d52a2e547ee980b29738d2690dfc8ea216e28021e9e568b9ae4d3651290aafb" alt=""><figcaption></figcaption></figure>

A new window will open prompting you to provide a gateway name and select the gateway’s access level.

Gateway names are subject to the same naming restrictions as bucket names. All gateway names must be lowercase, between 3-63 characters, and must be unique.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FvgYmcTAcbBuZKUp1eTlp%252Fimage.png%3Falt%3Dmedia%26token%3D57ea0989-e0f7-4799-b2f9-5cf3f4b1547c&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=f6cfa19ae178610b7d6e64e3a889eaec7202dc10a3aeb787f187fbbc47c13227" alt=""><figcaption></figcaption></figure>

Gateways can be public, private, or scoped.

#### Public Gateways <a href="#public-gateways" id="public-gateways"></a>

To create a public gateway, select ‘Public’. This can be changed after the gateway has been created.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FxShrYXtcI66ecdDIgKZh%252Fimage.png%3Falt%3Dmedia%26token%3D0417462b-e830-4003-8f09-fb6323ad4d8b&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=b9a4d20c341585c2145dd4ca6706f58fd25e6a2364b7a2b7604e50611ad46963" alt=""><figcaption></figcaption></figure>

#### Private Gateways <a href="#private-gateways" id="private-gateways"></a>

To create a private gateway, select ‘Private’. This can be changed after the gateway has been created.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FWXH7N1rpi5RyDS3o2DSv%252Fimage.png%3Falt%3Dmedia%26token%3D99aedf7d-62a0-4520-a496-07f8b3405bc8&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=01641a6eba94b1dedd8042030bf4cb8c23e89a34afe1aedd086ed547ba31447f" alt=""><figcaption></figcaption></figure>

#### Scoped Gateways <a href="#scoped-gateways" id="scoped-gateways"></a>

To create a scoped gateway, select ‘Private’, then select a bucket name from the drop-down menu for the scoped gateway to serve. Scoped gateways only serve content located in the bucket that they are restricted to.

Note: If a gateway is configured to serve a root CID, it cannot also be configured to be restricted to a bucket. The root CID configuration must be cleared to configure a bucket restriction.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FhyFgtrBcN5ZvusIkRWkg%252Fimage.png%3Falt%3Dmedia%26token%3D0266f9ad-0b05-41ff-a3a1-7ce7a2d75245&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=aa96520e6b2756e96284101c9b4cbb3a3feb4ec804142ef5976ad49072378455" alt=""><figcaption></figcaption></figure>

Alternatively, if you want to set a bucket restriction for a gateway that was previously created, you can set the restriction by navigating to the [Buckets](https://console.filebase.com/buckets) menu and selecting the three menu dots for the bucket you’d like to restrict your gateway to. Then select ‘Set Restriction’.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FWiPBDN8IZmhedRpnYDLs%252Fimage.png%3Falt%3Dmedia%26token%3D65418f22-c4a0-494a-9ba3-3f78148e3dd4&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=f9ffab2b9667f9fbcefc2b530c2e9fabc1c40ac6850d6f231fcfe13a22b20175" alt=""><figcaption></figcaption></figure>

When prompted, select the gateway you want configured to use the selected bucket.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FnM12qeXPePXz7jdCkDgZ%252Fimage.png%3Falt%3Dmedia%26token%3D37dab6c6-b0c9-45a6-88ab-a5ed58e409af&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9e809ca781b943b378693588c74e697bc7f3d345a1d1d31afe6f3070185bf3c3" alt=""><figcaption></figcaption></figure>

#### Removing a Bucket Restriction <a href="#removing-a-bucket-restriction" id="removing-a-bucket-restriction"></a>

To remove the bucket restriction on a gateway, navigate to the [Gateways](https://console.filebase.com/gateways) page, select the three menu dots on the right-hand side, then select ‘Clear Bucket Restrictions'

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FSr6r9XYSP0AhwlHjpsmO%252Fimage.png%3Falt%3Dmedia%26token%3Ddf5888a5-6c60-4e56-87dc-792f55cec442&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=6f1c8e45b6269ba1f43b01e12b628695e35686db850689305d430b989fc85124" alt=""><figcaption></figcaption></figure>

You will be prompted to confirm the removal.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FRyHpkZzaJJGkq8lkWDgy%252Fimage.png%3Falt%3Dmedia%26token%3D548933b3-f813-4b69-9c95-9d82f404626d&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3d1cf86672b5c85d4d39dc0344917189093cef7a9971029138db3d0acf0daad1" alt=""><figcaption></figcaption></figure>

#### Interacting With Gateways <a href="#interacting-with-gateways" id="interacting-with-gateways"></a>

Once a gateway has been created, a subdomain record is created that points to your dedicated gateway. For example, the dedicated gateway called 'documentation' will use the gateway URL:

[https://documentation.myfilebase.com/](https://documentation.myfilebase.com/)

**Toggling Private/Public Access**

To toggle between public and private access for a dedicated gateway, use the toggle switch that functions identically to the public and private access toggle switch for buckets.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F1Pq1Op5ooxK4EjevYLKu%252Fimage.png%3Falt%3Dmedia%26token%3Dd2247f40-d568-4189-9f2e-c7695ea70fcf&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c6b196e3a01b37296aae921cee93cc118ed99d57bd24134f8d62a20fa693cbad" alt=""><figcaption></figcaption></figure>

To interact with the gateway, you can select the three menu dots to open a list of options.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252Fsop5QWeKL58LKMWdT0Qh%252Fimage.png%3Falt%3Dmedia%26token%3D456d1a98-f88e-40cb-a9af-49bf3b2108da&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=44de6919bd89625158e6030826b957645a4e2f2fa2fb1fe3994690867048c9c9" alt=""><figcaption></figcaption></figure>

To open the gateway URL, select ‘Open’.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FMJChoWI6hEJ0Ih0PzFb3%252Fimage.png%3Falt%3Dmedia%26token%3D62e4576e-dd9e-46bc-a971-22a3c1b1663b&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=773b78976dd51ddd61a47bf091255ef47022512f868cc047444a59f28fc7dd58" alt=""><figcaption></figcaption></figure>

By default, the URL will return the following webpage:

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FiKCzkfQFFWVTZ4hyXmag%252Fimage.png%3Falt%3Dmedia%26token%3D26a9fe68-3ab1-4258-9261-1f6573d0ead1&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=30d0c8e88771442b079d92bb12ec34a6bef48e0f3f27d8e5a2d290c840636539" alt=""><figcaption></figcaption></figure>

This webpage is returned since there is no CID included in the URL. However, gateways can be configured to serve a CID or file as its root. This means instead of this default webpage, you can configure a different default static webpage or another file to be viewed by default rather than this error message.

This is most commonly used to serve static websites from a domain. Using this feature, [http://documentation.myfilebase.com/](http://documentation.myfilebase.com/) would return the file you selected, without having to enter a CID or path in the URL.

## Setting a CID as the Root of the Dedicated Gateway <a href="#setting-a-cid-as-the-root-of-the-dedicated-gateway" id="setting-a-cid-as-the-root-of-the-dedicated-gateway"></a>

Note: If a gateway is configured to be restricted to a bucket, it cannot be configured to have a root CID. The bucket restriction configuration will need to be cleared before a root CID can be configured for the gateway.

To configure this, navigate to the [Buckets](https://console.filebase.com/buckets) menu, and select an IPFS bucket.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F1e43jgN89lV7k3aTmRQe%252Fimage.png%3Falt%3Dmedia%26token%3Dd042d300-e08a-4a31-879a-0f36d13cde2d&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=695f5854490d8507cf5b1b68d8dd79998ea7f96ff70330faada1d9db55d4af97" alt=""><figcaption></figcaption></figure>

Once inside the bucket, select the file you’d like to set as the root file by selecting the three menu dots on the right-hand side.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FiIMjm3098ilorlB2eQ2F%252Fimage.png%3Falt%3Dmedia%26token%3Dcbe480f8-867a-4309-ae63-10d9ff979a18&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=caab3c29f29d12c55844cf39337e8aba3dae396254953becbac0099bf1bbcbfb" alt=""><figcaption></figcaption></figure>

Select ‘Set as Root’.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FDVnPyqH8ZmCTY1Huzzbf%252Fimage.png%3Falt%3Dmedia%26token%3De9325e91-665a-465e-925c-90c928b07269&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=6901581960d1e9875ba14c5513cce39a58ec2f3b2e9d155df1fe06c2483e7565" alt=""><figcaption></figcaption></figure>

Then choose the dedicated gateway you’d like to use.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FNP0O5wEUv0mjeTy7wS25%252Fimage.png%3Falt%3Dmedia%26token%3D10e269b4-449b-4e08-bad9-74665a19933b&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3aaf822f372eef37e88af216376a36e11911cb2b46a63416296a725fb55c4790" alt=""><figcaption></figcaption></figure>

Now, when you open the gateway, you’ll see the file you set as the root file displayed rather than the default error message.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252Fr4ZpQMnowQcazOAgPWKr%252Fimage.png%3Falt%3Dmedia%26token%3D5cb4c98b-6caf-4ee2-8750-7a7ae6c6e9c6&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=f0cac25e359fb89ae9e08a1038993194e03cf3ff3e69c45cbeeec335dc3766a6" alt=""><figcaption></figcaption></figure>

#### Removing a Root CID From a Gateway <a href="#removing-a-root-cid-from-a-gateway" id="removing-a-root-cid-from-a-gateway"></a>

To remove a CID set as the root CID for a dedicated gateway, navigate to the [Gateways](https://console.filebase.com/gateways) page, then select the three menu dots and select ‘Clear Root CID’.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FxjHb73SrHMddAgWkFkQf%252Fimage.png%3Falt%3Dmedia%26token%3D6f9872fa-80ca-42aa-9a7a-cf9dbdd3c2cf&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8d985c07e6ae06e4fbc17dde2d78ec909a89a996a62386a502de096ac36fe688" alt=""><figcaption></figcaption></figure>

You will be prompted to confirm the removal.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F7uz0V1HQGXz15lqnbMUY%252Fimage.png%3Falt%3Dmedia%26token%3D5b2cf394-eb53-44a8-a8c5-e9f02a764d74&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=964e154f6ebe24242ea063534a02020471a94b416d07ebb65c4ad2bff67df414" alt=""><figcaption></figcaption></figure>

#### Deleting a Gateway <a href="#deleting-a-gateway" id="deleting-a-gateway"></a>

To delete a dedicated gateway, navigate to the [Gateways](https://console.filebase.com/gateways) page in the Filebase web console dashboard.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FNRQnPAj7D4tTIST5HI8O%252Fimage.png%3Falt%3Dmedia%26token%3Dbde55526-be7e-409b-a37e-5e4e5a9d0360&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8c2dae0e377b7dfb5422d6f4efd153ec0c9d9ae3ef26958ddab203efb2e7f8ef" alt=""><figcaption></figcaption></figure>

Select the options menu by clicking the three dots on the right-hand side corresponding with the gateway you want to remove. Then select ‘Delete’.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FwJoygiG36m1GLNA4FZpK%252Fimage.png%3Falt%3Dmedia%26token%3Dc83acdf0-85af-4a5f-b5e0-162476b6a814&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c7bbdca1195f23343e8ef95cd011dbe98b0968be6a236649d6d31c9f4306acc6" alt=""><figcaption></figcaption></figure>

You will be prompted to confirm the removal.

### **Filebase IPFS Image Optimization** <a href="#filebase-ipfs-image-optimization" id="filebase-ipfs-image-optimization"></a>

Filebase provides an image optimization functionality directly through the Filebase IPFS dedicated gateway feature.

Through this feature, image load time and the overall image experience can be improved.

Any image file that is uploaded to Filebase can be manipulated through query string parameters. ‌The query string options are defined as follows.

**Image Optimization Options ‌**

To utilize image optimization, at least one option must be specified.

These parameters can be added to the URL of the image, file. For example:

If the default image URL is:

`https://documentation.myfilebase.com/ipfs/QmVnf5PnSUvjrPkc9tDgpwqcreKWh7xVyXDwDmS6xwchWp`

#### Filebase IPFS Image Optimization

Filebase offers image optimization through its IPFS gateway. You can manipulate image files using query string parameters:

*   **Resize**:

    ```css
    ?img-width=300
    ```
*   **Quality**:

    ```css
    ?img-quality=75
    ```
*   **Format**:

    ```arduino
    ?img-format=auto
    ```

For example:

```arduino
https://documentation.myfilebase.com/ipfs/QmVnf5PnSUvjrPkc9tDgpwqcreKWh7xVyXDwDmS6xwchWp?img-width=300
```

You can [sign up](https://filebase.com/signup) for a free Filebase account to get started with IPFS today.
