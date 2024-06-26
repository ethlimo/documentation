# .ART Resolution

The eth.limo gateway now supports [`.art`](https://protocol.art/) name resolution. This means that users can use the eth.limo gateway service for resolving and retrieving the `contenthash` records of `.art` domain names.

### Prerequisites

1. You must posses or control the `.art` domain NFT.
2. DNS integration must be enabled and configured. 
3. A valid `contenthash` record must exist for the `.art` domain.

Please consult the official documentation for any [questions](https://docs.protocol.art/.art-all-about-ens-mint).

### Configuration

Setting up your `.art` domain for LIMO resolution is an easy and straight forward process:

1. Login to the control panel of the registrar that you used to enable the DNS integration for your `.art` domain.
2. Identify the domain or subdomain that you intend to use.
3. Configure the following `A` records for your domain(s) or subdomain(s):
  ```
3.140.119.203
18.219.255.217
3.135.72.151
  ```

_Note_ - a DNS `A` record can contain multiple IP addresses. Setup and workflow steps may differ between registrars. Please consult the provider's documentation for your specific use case.

4. Verify that your `.art` domain is configured correctly and that the record updates have propagated through the DNS network. The steps below should work for any domain, however for the purposes of this documentation we will be using `limo.art`:

```shell
$ dig limo.art +short

3.140.119.203
18.219.255.217
3.135.72.151
```

5. The IP addresses returned by the command above should match those of step 3. Once you have confirmed that the record is properly configured you're all done!
6. Navigate to your domain and watch as your dWebsite content is resolved from your `contenthash`.

You will need to repeat the steps above for any additional subdomains that you wish to use. All names must have a valid resolver and `contenthash` record - alternatively you can also use CCIP integrations just like standard `.eth` domains.