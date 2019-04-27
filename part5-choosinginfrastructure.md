# Part 5: Choosing a Cloud Infrastructure Provider

There are numerous "Infrastructure-as-a-Service" \(IaaS\) providers and many of them are sufficient for our needs. The requirements for this guide are rudimentary, so your selection may come down to personal preference or geography.

Any commercial provider can support a Docker swarm, because there aren't really any special requirements to run Docker. That's not to say that all providers are created equal; quality of life and ease of administration perks can make a huge difference in the complexity of your DevOps environment. Your individual applications may dictate a valuation for some of the below perks that is different than our classification, and that's as it should be! For a generic pipeline, we'll emphasize the features that have contributed the most to making our jobs easier.

## Perks: Quality of Life and Ease of Administration

1. **Internal Networking**. This is our favorite perk: we wouldn't select a hosting provider for a small-to-medium size shop unless we can count on routing all our internal traffic on a network other than the public Internet. It simplifies firewall rules and doesn't clutter our bandwidth metrics. 
2. **Floating IPs**: Having a fixed set of reserved IP addresses will simplify deployment and failover: if we need to upgrade our database or a service that isn't Docker-ized, we can spin up a new instance, test everything, and then re-point the destination address from the previous host to the new one. Better still if this feature is combined with private networking and you can have floating IPs on internal network addresses.
3. **Detachable** **Block Storage:** While our code repositories will live in the Docker images on our cloud computing instance, it's useful to have static storage if our apps store files or media. With the proliferation of S3-compatible storage providers and CDNs, it's not essential to have detachable storage on the provider level, but sometimes you just need a place to put some files.  Think of **Block Storage** like an external drive attached to your instance: it's separate, but can only be attached to one instance at a time. It's possible to manage without detachable block storage; you could, for example, simulate it by having a second VM with a networked file system, but dedicated block storage is likely to be faster and definitely easier to deal with. In this guide, we'll use block storage both for our database file storage and for a network file share that will do double duty as a low-rent persistent storage layer for our containers.
4. **Automated Backups**. While your backup strategy is likely to rely on additional elements beyond snapshots of your host \(particularly for your database\), regular snapshots add a valuable layer of insurance and protection. We prefer to run our own backup solution; there are so many options and storage is cheap.
5. **Provider-level Firewall**. While we will make use of OS-level firewalls like the [uncomplicated firewall](https://wiki.ubuntu.com/UncomplicatedFirewall), we have less to worry about if we can also apply templates defined at the provider level to our instances. Rapidly applying firewall rules from templates is a big time-saver.
6. **Load Balancing**. Even if you're not yet concerned with scaling to handle heavy load, a provider that offers automated balancing between instances will make this a trivial task when the time comes. It's also straightforward to "roll your own" load balancer with nginx or traefik, though, so it's not essential that your host have an off-the-shelf solution.
7. **Object Storage**. Object storage is great for storing anything that doesn't need the low latency of a local filesystem. Amazon's S3 is the most common example, but there are several S3-compatible providers and some hosts have their own, like DigitalOcean Spaces. Of course, you can always just use S3 no matter which provider you use; the only advantage to marrying your object storage provider to your hosting provider is that you may be able to dodge bandwidth usage when your own containers are sending or receiving files.

## Contenders: DigitalOcean, Vultr, Amazon, Google Cloud, Or Anything Else

The point of this guide is not to anoint one or more providers as the best for CF cloud deployment. There are so many providers in the market at similar price points that the "best practice" will come down to your needs and preferences. For our purposes, we've selected two providers based on our own research and recommendations from established users. While this guide will use their terminology and screenshot their utilities, almost all of the preparation and configuration will be similar \(control panels, networking, and provisioning\) or identical \(Docker configuration\).

### First Prize: DigitalOcean

![](https://github.com/MordantWastrel/cf_swarm/tree/09923ffa6d5073e322f55f47dc8cbdbfde1b3f3f/part5-choosinginfrastructure/.gitbook/assets/digitalocean_1_390x195.png)

DigitalOcean offers cloud compute instances \(called "droplets"\) with every one of the quality of life perks from our list. While DigitalOcean is geared toward Linux deployments, as of October 2018 they now support custom installation images, so you can set up a Windows "droplet" with your own licensing and a little bit of creativity. 

### Windows Hosting: Vultr

![](https://github.com/MordantWastrel/cf_swarm/tree/09923ffa6d5073e322f55f47dc8cbdbfde1b3f3f/part5-choosinginfrastructure/.gitbook/assets/vultr-vps-review_00-300x190.jpg)

Vultr's offers compute instances with similar configurations and pricing to DigitalOcean, but they also offer licensed instances for Windows Server OS, dedicated instances, non-virtualized baremetal service, and block storage.

Vultr also offers several distributions of Linux, along with the ability to upload your own ISO, so it is by no means just for Windows users.

### Honorable Mention: AWS / Amazon Lightsail, Linode, Google Cloud

There are numerous other providers and if you already have one you like, you should stick with them provided that they meet the requirements and offer most of the perks from our list. [Linode](https://www.linode.com/) is a popular alternative, and there are [articles](https://www.mamboserver.com/digitalocean-alternatives/) published once or twice a year with a round-up of the market.

If you already have experience or an investment in AWS or Goolge Cloud, you have everything you need to proceed and there is likely no reason to start with a new provider. AWS is not used in this guide is because the Amazon ecosystem does so much more than we need for even a well-developed application development CF pipeline that the added learning curve puts it just out of reach for this guide. We found Google Cloud to have slightly less of a learning curve, but its pricing was not competitive with DigitalOcean or Vultr as of March 2018 unless you commit in advance to monthly service.

An exception is [Amazon Lightsail](https://aws.amazon.com/lightsail/), which is a simplified offering based on specific AWS compute instances and some provider-level tools \(and pricing\) comparable to DigitalOcean and Vultr. If you know you're going to end up in the AWS ecosystem, consider starting with Lightsail rather than DigitalOcean or Vultr; like Vultr, Lightsail offers Windows instances. Amazon also offers block storage through their [Elastic Block Storage](https://aws.amazon.com/ebs/pricing/) service at a price point comparable to DigitalOcean. Google Cloud also has both Windows instances and block storage.

