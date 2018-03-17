# Your First Instance: Provisioning

If you're already familiar with provisioning and configuring an instance \(droplet\) and just want to get to Docker, you can skip this section.

Whether you're setting up your database server, a Docker server, or most anything else, the initial setup is the same -- enough so that we'll create a snapshot once we're done and use that as our starting point for future droplets.

* Login as Root; add a new user for yourself and configure Root privileges
* Add Public Key authentication
* \(Optional\) Disable password logins
* Basic Firewall Configuration
* VPN Configuration

## Create a Droplet

When you log in to your DigitalOcean account, you'll be greeted with an empty Droplet Management utility.

> #### Aside: Billing Expectations
>
> You will have supplied your provider with billing information by this stage so that you can spin up instances. Depending on how quickly you get through this guide, the total cost should be between **$5** and **$20**, depending on how diligent you are about taking snapshots and destroying your instances at the end of each day. So long as you've made it through the basic security section, you can save a 5-6 minutes each day by skipping the snapshot/destroy/re-create process for each of your instances.

![](/assets/snip_20180317095854.png)

Select **Create Droplet** and let's deploy our first instance.

### ![](/assets/snip_20180317103147.png)

> #### Aside: Distributions and One-Click Apps
>
> Many providers offer "one-click app" images -- instances that come with flagship applications like MySQL, Docker, NodsJS, or Wordpress already installed. Since this guide is intended to familiarize you with every stage of the development process, we're going to use the basic **distribution** images and install our own apps, but once you know what you're doing, one-click images can save you some time.

## Selecting a Distribution and Instance Size

Keep the default selection of Ubuntu \(16.04 4 as of March 2018\). Select a **4 GB / 2 vCPU** instance size.

> #### Aside: Smaller Instance Sizes
>
> While you can set up MySQL, MSSQL, Docker, and CF on instances with less RAM, saving between **.8** and **1.5** cents per hour \(or **$5/mo\)** is not really worth sacrificing the resources -- in some cases, your services will need extra configuration to run with low memory, and we prefer "fast and simple" over having to add configuration that we'd then have to pull out when scaling up later anyway.
>
> #### Aside: Distributions Other than Ubuntu
>
> We use Ubuntu in this guide, but you can run everything you need for a CF pipeline on most distributions. If you select a distribution other than Ubuntu, check your provider or Google for links to configuration guides specific to your distribution.



