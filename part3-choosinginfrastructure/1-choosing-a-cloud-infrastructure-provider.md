# Part 3: Choosing a Cloud Infrastructure Provider

There are numerous "Infrastructure-as-a-Service" \(IaaS\) providers and many of them are sufficient for our needs. The requirements for this guide are rudimentary, so your selection may come down to personal preference or geography.

Let's have a look at what we need, and then consider a few perks that will make our job easier.

## Requirements: What We Need in our Host

1. **Detachable** **Block Storage**. While our code repositories will live in the Docker images on our VM host, we need someplace for static storage if our apps store files or media. Even if you keep files or archives on a separate provider like Amazon S3, it's still a good idea to have locally-accessible but persistent storage. It's possible to manage without separate and detachable block storage; you could, for example, simulate it by having a second VM with a networked file system. This guide will assume that you can mount such a volume as a trivial matter.
2. **Docker**. You must be able to run Docker. Since Docker runs on most everything, this is an easy hurdle to clear. Most hosting providers offer VMs with Docker pre-installed.

## Perks: Quality of Life and Ease of Administration

Some providers offer additional features that can make our job easier. Several of these are system or network administration tasks that you can perform on any host but which are made far easier when available in a consistent or automated fashion.

1. **Floating IPs**: Having a fixed set of reserved IP addresses will simplify deployment and failover -- if we need to upgrade our database or a service that isn't Docker-ized, we can spin up a new VM, test everything, and then re-point the destination address from the previous host to the new one.
2. **Automated Backups**. While your backup strategy is likely to rely on additional elements beyond snapshots of your host \(particularly for your database\), regular snapshots add a valuable layer of insurance and protection.
3. **Provider-level Firewall**. While we will make use of OS-level firewalls, we have less to worry about if we can also apply templates defined at the provider level to our VMs.
4. **Private Networking**. We can set up an internal VPN or firewall rules to enable our hosts to communicate with one another, but much like a provider-level Firewall, it will save us some time and complexity if it's already done for us.
5. **Load Balancing**. Even if you're not yet concerned with scaling to handle heavy load, a provider that offers automated balancing between VMs will make this a trivial task when the time comes.



