# Part 2: Before You Begin

One of the great things about containerized deployments is that you don't need very much to get started. The whole idea behind containers is that they're standardized, so even though you may have some customization to perform, you're not going to be reinventing any wheels.

## Requirement \#1: A Computer and an Internet Connection

Neither of them have to be particularly good -- the heavy lifting will be done in the cloud. The requirements for local development \(running a CF engine and a database\) are greater than the requirements for managing your pipeline. For the purposes of this guide, we'll assume you have a local machine that can run your favorite CF Engine and RDBMS.

## Requirement \#2: A Small Amount of Money

Most of our familiarity with Docker and CF will come in the next section, **Docker in Development**, and that will take place on your local machine. Once we graduate to Docker in Staging and Production, all of the action takes place at your cloud infrastructure provider, and most of them bill for hourly usage. Your usage will not be very high, but you'll need a credit card on file with your provider. The cloud computing instances we'll be using to get started in this guide \(as of June 2018\) cost at most **30 cents/hour** or **$20/month**, and you'll be able to take snapshots at the end of the day and destroy the instance so that you are only paying for the time you're working through this guide. Snapshot storage at the provider we use is **5 cents per gigabyte per month**.

## Requirement \#3: Linux for Production, Whatever for Development

For production deployments, the current version of this guide targets Linux and uses NGINX as a front-end web server. For Development, any platform capable of running Docker will do. Windows users should be running Linux Containers and a version of Windows with Hypervisor \(e.g. Windows 10 Pro or Enterprise\); Docker Toolbox is an alternative if you can't or don't want to run Hyper-V, but some of the networking values will be different and the guide does not currently cover Docker Toolbox.

While it is possible to get a cloud Windows instance and run Docker on Windows \(and therefore to accomplish everything in this guide using Windows and IIS\), as yet the authors are not aware of anyone that has actually done so with a Coldfusion pipeline. Theoretically, the only difference is in the Docker configuration portion, and that you're configuring IIS rather than NGINX. Both Windows Server 2019 and Java have prioritized Docker support, so we would expect that it will only get easier to maintain a Windows-based CF pipeline. We recommend making use of [Bilal's Boncode AJP Tomcat Connector](http://boncode.net/connector/webdocs/Tomcat_Connector.htm) for IIS regardless of whether you use Adobe CF or Lucee; the configuration footprint is less than Adobe's ISAPI-based connectors \(making containerization much easier\) and it performs very well besides.

When possible, we include references to Windows resources \(including a cloud infrastructure host that offers them\) and we invite anyone who has been through this process with Windows and IIS to contribute to the guide.

## Requirement \#4: A \(Reasonably\) Modern CF Engine; preferably, Lucee 5+

The glue that holds all the pieces together in any CF deployment is Commandbox with the [CFConfig](https://www.forgebox.io/view/commandbox-cfconfig) module. As of December 2019, CFConfig supports the following CF engines:

* Adobe Coldfusion 9\*
* Adobe Coldfusion 10\*
* Adobe Coldfusion 11\*
* Adobe Coldfusion 2016\*
* Adobe Coldfusion 2018\*
* Lucee 4.5
* Lucee 5
* Railo 4.x

Unfortunately, there's a little more to our choice of application engine than CFConfig support. 

> **\*Adobe Coldfusion Note**
>
> It was the authors' intention that this guide take a vendor-neutral approach, even if we might point out an occasional advantage or shortcoming here and there. Unfortunately, there are several "gotchas" to using the Adobe Coldfusion engine for modern development, whether in Docker or not; some of those gotchas are particular to Docker, but ultimately, **we cannot recommend that any institution or enterprise use Adobe Coldfusion:**

> **1) Standard licenses cannot be used in production with Commandbox. **As of December 2019, running any version of Adobe Coldfusion through Commandbox will cause CF to detect a J2EE/WAR deployment which is only supported in the Enterprise edition. This issue was [raised during the Public Beta for CF 2018](https://coldfusion.adobe.com/discussion/2479279/) but Adobe declined to address it. They had previously suggested that WAR deployments would not automatically be forced into Enterprise mode for 2018 but that does not appear to have materialized.

>Furthermore, even if you do have an Enterprise license, you are limited to 8 containers per license. See [Adobe Coldfusion Docker Licensing](https://coldfusion.adobe.com/2019/03/coldfusion-licensing-docker-containers/) for more information. (Early indications are that friendlier terms may be coming with the 2020 release; but we've seen early indications not materialize in the final product before.)
>
> **2) Adobe has developed a unique definition of "Software as a Service" that they are employing to demand additional license fees from customers already paying for Enterprise licenses.** This kerfuffle is described in detail in [this Adobe forum thread](https://forums.adobe.com/thread/2648828). The discussion is beyond the scope of this guide, but a conservative takeaway is that the licensing landscape for Adobe Coldfusion is quite complicated, and there are even indications that Adobe is threatening customers that migrate to Lucee with retroactive SaaS license payment demands. 

> Our experience with CF and Docker is almost entirely on the Lucee side, and even were Adobe's licensing positions clearer and friendlier, there just aren't any compelling reasons to prefer Adobe's solution going into 2020. If your business environment requires the use of Adobe Coldfusion, much of this guide will still apply, but we recommend stopping by the CFML Slack (cfml.slack.com) and visiting #docker for some up-to-date information.

> Additionally, this guide is all about Commandbox deployments \(for now\) but Adobe provides [official docker images](https://bintray.com/eaps/coldfusion) for CF 2016 and 2018; the process for building the images is just different \(and a bit less convenient\) because you'll have to edit XML files or else build Commandbox and CFConfig into your image process but not actually run your server with them.
>
> For these reasons, the recommended engine for this guide is Lucee. This is primarily a production consideration; the author used ACF11 with Docker in development for &gt; over a year and it is easy to get Adobe Coldfusion working for development or staging environments where licensing isn't an issue.



