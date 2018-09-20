# Part 2: Before You Begin

One of the great things about containerized deployments is that you don't need very much to get started. The whole idea behind containers is that they're standardized, so even though you may have some customization to perform, you're not going to be reinventing any wheels.

## Requirement \#1: A Computer and an Internet Connection

Neither of them have to be particularly good -- the heavy lifting will be done in the cloud. The requirements for local development \(running a CF engine and a database\) are greater than the requirements for managing your pipeline. For the purposes of this guide, we'll assume you have a local machine that can run your favorite CF Engine and RDBMS.

## Requirement \#2: A Small Amount of Money

Most of our familiarity with Docker and CF will come in the next section, **Docker in Development**, and that will take place on your local machine. Once we graduate to Docker in Staging and Production, all of the action takes place at your cloud infrastructure provider, and most of them bill for hourly usage. Your usage will not be very high, but you'll need a credit card on file with your provider. The cloud computing instances we'll be using to get started in this guide \(as of June 2018\) cost at most **30 cents/hour** or **$20/month**, and you'll be able to take snapshots at the end of the day and destroy the instance so that you are only paying for the time you're working through this guide. Snapshot storage at the provider we use is **5 cents per gigabyte per month**.

## Requirement \#3: Linux for Production, Whatever for Development

For production deployments, the current version of this guide targets Linux and uses NGINX as a front-end web server. For Development, any platform capable of running Docker will do. Windows users should be running Linux Containers and a version of Windows with Hypervisor (e.g. Windows 10 Pro or Enterprise); Docker Toolbox is an alternative if you can't or don't want to run Hyper-V, but some of the networking values will be different and the guide does not currently cover Docker Toolbox.

While it is possible to get a cloud Windows instance and run Docker on Windows \(and therefore to accomplish everything in this guide using Windows and IIS\), as yet the authors are not aware of anyone that has actually done so with a Coldfusion pipeline. Theoretically, the only difference is in the Docker configuration portion, and that you're configuring IIS rather than NGINX. Both Windows Server 2019 and Java have prioritized Docker support, so we would expect that it will only get easier to maintain a Windows-based CF pipeline. We recommend making use of [Bilal's Boncode AJP Tomcat Connector](http://boncode.net/connector/webdocs/Tomcat_Connector.htm) for IIS regardless of whether you use Adobe CF or Lucee; the configuration footprint is less than Adobe's ISAPI-based connectors \(making containerization much easier\) and it performs very well besides.

When possible, we include references to Windows resources \(including a cloud infrastructure host that offers them\) and we invite anyone who has been through this process with Windows and IIS to contribute to the guide.

## Requirement \#4: A \(Reasonably\) Modern CF Engine

The glue that holds all the pieces together in any CF deployment is Commandbox with the [CFConfig](https://www.forgebox.io/view/commandbox-cfconfig) module. As of March 2018, CFConfig supports the following CF engines:

* Adobe Coldfusion 9\*
* Adobe Coldfusion 10\*
* Adobe Coldfusion 11\*
* Adobe Coldfusion 2016\*
* Adobe Coldfusion 2018\*
* Lucee 4.5
* Lucee 5
* Railo 4.x

> **\*Adobe Coldfusion Note**
>
> As of August 2018, running any version of Adobe Coldfusion  through Commandbox  will cause CF to detect a J2EE/WAR deployment which is only supported in the Enterprise edition. Furthermore, even if you do have an Enterprise license, Adobe's licensing scheme (even for 2018) is "one license, one container." This issue was [raised during the Public Beta for CF 2018](https://coldfusion.adobe.com/discussion/2479279/) but Adobe declined to address it. They had previously suggested that WAR deployments would not automatically be forced into Enterprise mode for 2018 but that does not appear to have materialized.
>
> This doesn't mean you can't run Adobe Coldfusion in Docker. It does mean that you can't use Commandbox to deploy it unless you have an Enterprise license. This guide is all about Commandbox deployments (for now) but Adobe provides [official docker images](https://bintray.com/eaps/coldfusion) for CF 2016 and 2018; the process for building the images is just different (and a bit less convenient) because you'll have to edit XML files or else build Commandbox and CFConfig into your image process but not actually run your server with them.
>
> For these reasons, the primary recommended engine for this guide is Lucee 5. This is primarily a production consideration. It's quite easy to get ACF 11 or later  working for development or staging environments where licensing isn't an issue.

