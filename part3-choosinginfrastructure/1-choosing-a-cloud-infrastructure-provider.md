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
6. **Object Storage**. While we'll want to keep our databases and web site media on block storage, object storage is great for storing anything that doesn't need the low latency of an enterprise application. Amazon's S3 is the most common example.

## Contenders: DigitalOcean, Vultr, Amazon, Or Anything Else

The point of this guide is not to anoint one or more providers as the best for CF cloud deployment. There are so many providers in the market at similar price points that the "best practice" will come down to your needs and preferences. For our purposes, we've selected two providers based on our own research and recommendations from established users. While this guide will use their terminology and screenshot their utilities, almost all of the preparation and configuration will be similar \(control panels, networking, and provisioning\) or identical \(Docker configuration\).

## Linux Hosting: DigitalOcean

### ![](/assets/digitalocean_1_390x195.png)

DigitalOcean offers virtual machines \(called "droplets"\) with every one of the quality of life perks from our list.  All of the examples for the Linux sections of this guide will use DigitalOcean. 

### Windows Hosting: Vultr

### ![](/assets/Vultr-VPS-Review_00-300x190.jpg)

Vultr's offers VMs with similar configurations and pricing to DigitalOcean, but they also offer Windows Server OS \(2012 and 2016\) . The only drawback to Vultr is that, as of March 2018, their block storage instances are regularly sold out. As an alternative, you can provision a separate cloud computing \(VC2\) instance just for storage.

Vultr also offers several distributions of Linux, along with the ability to upload your own ISO, so it is by no means just for Windows users.

### This Guide is Linux-Only ... For Now

The current version of this guide does not cover a CF cloud deployment running on Windows and IIS, but only because the bulk of the authors' and contributors' experience with cloud hosting and Docker is using Linux. 

