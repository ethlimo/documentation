# Gateway Basics

### Caching

By default all content is cached for 15 minutes. This means that if you update the `contenthash` of your ENS domain, it _may_ take _up to_ 15 minutes for those changes to become visible. We are actively working on a refactor of the caching layer which will eventually deprecate this limitation.

### HTTPS Certificates

All subdomain certificates, i.e. non `*.eth.limo` are generated on demand, provided that there is a valid resolver set for the subdomain. Users no longer need to request certificates manually in our [Discord server](https://discord.gg/zf8NxW94rB).