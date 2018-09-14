# Part 3: Choosing a Cloud Infrastructure Provider

There are numerous "Infrastructure-as-a-Service" \(IaaS\) providers and many of them are sufficient for our needs. The requirements for this guide are rudimentary, so your selection may come down to personal preference or geography.

Let's have a look at what we need, and then consider a few perks that will make our job easier.

## Requirements: What We Need in our Host

1. **Detachable** **Block Storage**. While our code repositories will live in the Docker images on our cloud computing instance, we need someplace for static storage if our apps store files or media. Even if you keep files or archives on a separate provider like Amazon S3, it's still a good idea to have locally-accessible but persistent storage. Think of **Block Storage** like an external drive attached to your instance: it's separate, but can only be attached to one instance at a time. It's possible to manage without detachable block storage; you could, for example, simulate it by having a second VM with a networked file system, but dedicated block storage is likely to be faster and definitely easier to deal with. This guide will assume that you can mount such a volume as a trivial matter.
2. **Docker**. You must be able to run Docker. Since Docker runs on most everything, this is an easy hurdle to clear. Most hosting providers offer instances with Docker pre-installed.

## Perks: Quality of Life and Ease of Administration

Some providers offer additional features that can make our job easier. Several of these are system or network administration tasks that you can perform on any host but which are made far easier when available in a consistent or automated fashion.

1. **Floating IPs**: Having a fixed set of reserved IP addresses will simplify deployment and failover -- if we need to upgrade our database or a service that isn't Docker-ized, we can spin up a new instance, test everything, and then re-point the destination address from the previous host to the new one.
2. **Automated Backups**. While your backup strategy is likely to rely on additional elements beyond snapshots of your host \(particularly for your database\), regular snapshots add a valuable layer of insurance and protection.
3. **Provider-level Firewall**. While we will make use of OS-level firewalls, we have less to worry about if we can also apply templates defined at the provider level to our instances.
4. **Private Networking**. We can set up an internal VPN or firewall rules to enable our hosts to communicate with one another, but much like a provider-level Firewall, it will save us some time and complexity if it's already done for us.
5. **Load Balancing**. Even if you're not yet concerned with scaling to handle heavy load, a provider that offers automated balancing between instances will make this a trivial task when the time comes.
6. **Object Storage**. While we'll want to keep our databases and web site media on block storage, object storage is great for storing anything that doesn't need the low latency of an enterprise application. Amazon's S3 is the most common example.

## Contenders: DigitalOcean, Vultr, Amazon, Google Cloud, Or Anything Else

The point of this guide is not to anoint one or more providers as the best for CF cloud deployment. There are so many providers in the market at similar price points that the "best practice" will come down to your needs and preferences. For our purposes, we've selected two providers based on our own research and recommendations from established users. While this guide will use their terminology and screenshot their utilities, almost all of the preparation and configuration will be similar \(control panels, networking, and provisioning\) or identical \(Docker configuration\).

### Linux Hosting: DigitalOcean

![](/.gitbook/assets/digitalocean_1_390x195.png)

DigitalOcean offers cloud compute instances \(called "droplets"\) with every one of the quality of life perks from our list. All of the examples for the Linux sections of this guide \(which is currently all the sections\) will use DigitalOcean.

### Windows Hosting: Vultr

![](/.gitbook/assets/vultr-vps-review_00-300x190.jpg)

Vultr's offers compute instances with similar configurations and pricing to DigitalOcean, but they also offer Windows Server OS \(2012 and 2016\) . The only drawback to Vultr is that, as of March 2018, their block storage instances are regularly sold out. As an alternative, you can provision a separate cloud computing \(VC2\) instance just for storage, or else get on their notification list for storage instances -- you may only be waiting a couple of days.

Vultr also offers several distributions of Linux, along with the ability to upload your own ISO, so it is by no means just for Windows users.

### Honorable Mention: AWS / Amazon Lightsail, Linode, Google Cloud

There are numerous other providers and if you already have one you like, you should stick with them provided that they meet the requirements and offer most of the perks from our list. [Linode](https://www.linode.com/) is a popular alternative, and there are [articles](https://www.mamboserver.com/digitalocean-alternatives/) published once or twice a year with a round-up of the market.

If you already have experience or an investment in AWS or Goolge Cloud, you have everything you need to proceed and there is likely no reason to start with a new provider. AWS is not used in this guide is because the Amazon ecosystem does so much more than we need for even a well-developed application development CF pipeline that the added learning curve puts it just out of reach for this guide. We found Google Cloud to have slightly less of a learning curve, but its pricing was not competitive with DigitalOcean or Vultr as of March 2018.

An exception is [Amazon Lightsail](https://aws.amazon.com/lightsail/), which is a simplified offering based on specific AWS compute instances and some provider-level tools \(and pricing\) comparable to DigitalOcean and Vultr. If you know you're going to end up in the AWS ecosystem, consider starting with Lightsail rather than DigitalOcean or Vultr; like Vultr, Lightsail offers Windows instances. Amazon also offers block storage through their [Elastic Block Storage](https://aws.amazon.com/ebs/pricing/) service at a price point comparable to DigitalOcean. Google Cloud also has both Windows instances and block storage.

