# Updating Your ENS Content Records

### Go to the ENS Manager App <a href="#h_aeeb903a9a" id="h_aeeb903a9a"></a>

Go to the [ENS Manager App](https://app.ens.domains/) with the wallet set as Manager for the ENS name you want to manage and click Names to bring up a list of your ENS names _or_ search for an ENS name you own directly from the main page.

<figure><img src="../../.gitbook/assets/ens-manager-01.png" alt=""><figcaption></figcaption></figure>

Click the ENS name you want to add the content record to.

<figure><img src="../../.gitbook/assets/ens-manager-02.png" alt=""><figcaption></figcaption></figure>

Go to the Records tab

<figure><img src="../../.gitbook/assets/ens-manager-03.png" alt=""><figcaption></figcaption></figure>

Click Edit Records

<figure><img src="../../.gitbook/assets/ens-manager-04.png" alt=""><figcaption></figcaption></figure>

Then go to the Other tab

<figure><img src="../../.gitbook/assets/ens-manager-05.png" alt=""><figcaption></figcaption></figure>

Type in the IPFS CID you saved earlier into the Content field and then click Save\
​

The content hash field accepts any supported storage protocol — enter your identifier with the matching prefix:

| Protocol | Format | Where it comes from |
| -------- | ------ | ------------------- |
| IPFS | `ipfs://bafy...` | [Uploading to IPFS](../../beginner/how-to-use-ipfs-ipns/uploading-to-ipfs/README.md) |
| IPNS | `ipns://k51...` | [How to Publish to IPNS](../../ipns-publishing/how-to-publish-to-ipns.md) |
| Arweave / ArNS | `ar://<transaction-id>` | [Arweave and ArNS](../../advanced/arweave-arns.md) |
| Swarm | `bzz://<64-hex-reference>` | [Hosting on Swarm](../../swarm/hosting-on-swarm.md) |
| TON Sites | `adnl://<adnl-address>` | [TON Sites](../../ton/ton-sites.md) |

<figure><img src="../../.gitbook/assets/ens-manager-06.png" alt=""><figcaption></figcaption></figure>

Click Open Wallet and confirm the transaction in your wallet.

<figure><img src="../../.gitbook/assets/ens-manager-07.png" alt=""><figcaption></figcaption></figure>

Wait for the transaction to complete.

<figure><img src="../../.gitbook/assets/ens-manager-08.png" alt=""><figcaption></figcaption></figure>

Once the transaction has completed you should see a screen like this, it means that the changes to the content record is now stored on-chain. Click Done to go back to the Manager.

<figure><img src="../../.gitbook/assets/ens-manager-09.png" alt=""><figcaption></figcaption></figure>

You should now see the Content Hash record populated with the IPFS CID you entered.

<figure><img src="../../.gitbook/assets/ens-manager-10.png" alt=""><figcaption></figcaption></figure>

Congratulations! That's it! Now you can try visiting your website!

### ENS Compatible browsers[​](http://localhost:3000/howto/decentralized-website#ens-compatible-browsers) <a href="#h_c445e8ea66" id="h_c445e8ea66"></a>

For ENS resolution in the browser address bar to work, the browser needs to be compatible with ENS or have the Metamask extension installed.

Some browsers which are more oriented towards Web3 support ENS right out of the box:

* [Brave Browser](https://brave.com/)
* [Opera Browser](https://www.opera.com/)

Other browsers will need to have an extension installed for it to work, such as the Metamask browser extension:

* [Google Chrome](https://www.google.com/chrome)
* [Mozilla Firefox](https://www.mozilla.org/)

### ETH.LIMO - Web3 Gateway <a href="#h_b6b11193b5" id="h_b6b11193b5"></a>

You can also use the eth.limo web3 gateway by entering in your-ens-name.eth.limo into any browser address bar, regardless of if it supports ENS names natively or not.
