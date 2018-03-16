# Part 2: Before You Begin

This is the shortest section of the guide.

One of the great things about containerized deployments is that you don't need very much to get started. The whole idea behind containers is that they're standardized, so even though you may have some customization to perform, you're not going to be reinventing any wheels.

## Requirement \#1: A Computer and an Internet Connection

Neither of them have to be particularly good -- the heavy lifting will be done in the cloud. The requirements for local development \(running a CF engine and a database\) are greater than the requirements for managing your pipeline. For the purposes of this guide, we'll assume you have a local machine that can run your favorite CF Engine and RDBMS.

## Requirement \#2: A Small Amount of Money

Almost all of the action takes place at your cloud infrastructure provider, and most of them bill for hourly usage. Your usage will not be very high, but you'll need a credit card on file with your provider. The VMs we'll be using to get started in this guide \(as of March 2018\) cost at most **30 cents/hour **or **$20/month**, and you'll be able to take snapshots at the end of the day and destroy the VM so that you are only paying for the time you're working through this guide. Snapshot storage at the provider we use is **5 cents per gigabyte per month**.

## Requirement \#3: Linux \(...or Windows\)

The current version of this guide targets Linux and uses NGINX as a front-end web server.

While it is possible to get a Windows VM and run Docker on Windows \(and therefore to accomplish everything in this guide using Windows and IIS\), as yet the authors are not aware than anyone has actually done so. Theoretically, the only difference is in the Docker configuration portion, and that you're configuring IIS rather than NGINX. 

We will include references to Windows resources \(including a cloud infrastructure host that offers them\) and invite anyone who has been through this process with Windows and IIS to contribute to the guide.

## Requirement \#4: A \(Reasonably\) Modern CF Engine

The glue that holds all the pieces together in any CF deployment is Commandbox with the [CFConfig](https://www.forgebox.io/view/commandbox-cfconfig) module. As of March 2018, CFConfig supports the following CF engines:

* Adobe Coldfusion 11 or later
* Lucee 4.5
* Lucee 5

While it's possible to deploy Adobe Coldfusion 9 or 10 with Commandbox and find workarounds for using CFConfig, doing so is a tremendous amount of extra work and beyond the scope of this guide. If you need to deploy one of these engines, you may be better served [contacting Ortus](https://www.ortussolutions.com/#contact) and sponsoring official ACF9 or ACF10 support in CFConfig rather than trying to navigate hacking XML files into your Docker image.

Better yet, since you're already upgrading your infrastructure, take advantage of your new pipeline to upgrade your CF engine as well!

