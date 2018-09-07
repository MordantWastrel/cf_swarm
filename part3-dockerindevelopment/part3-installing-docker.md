## Get Docker
{% hint style='tip' %}
Local Installation and initial configuration (Time Required: 5 Minutes)
{% endhint %}
![Figure 3.2: The Docker Whale ](/assets/docker-whale.png)


Familiarity with Docker is not a pre-requisite for this guide; you can simply follow the instructions and you'll acquire a basic understanding of how Docker works just by seeing it in action. Nevertheless, if you are able to invest some time up front into learning about Docker generally, you will have an easier time in the long run -- particularly in the later, Production sections.

Our favorite Docker resources are:

* The [Udemy Docker Mastery course](https://udemy.com/docker-mastery/) by Bret Fisher -- it can be completed in a week or less and is one of the best and most accessible entry points for the Docker universe.
* The Ortus Container Roadshow from 2017, which covers containerizing CF more specifically
{% endhint %}

Start by installing [Docker CE](https://store.docker.com/search?type=edition&offering=community) \(Community Edition\) on your machine.  It doesn't matter whether you're using a Windows PC, a Mac, or Linux; get the version of Docker appropriate to your OS. For local development, we like to use the Edge channel, which is a monthly release that has the latest features; however, nothing in this guide requires those and the Stable channel is fine if you prefer.

Linux users can take advantage of the automated Docker install script at http://get.docker.com.


** Differences between Docker, Docker for Windows, and Docker for Mac**
Docker under Linux is just called "Docker" and refers to two pieces:
* The Docker daemon, running in a "native" environment where it can share resources directly with the host without running a virtual machine.
* The Docker client, which handles your requests from the command line by authenticating and communicating with the Docker daemon.

"Docker for Windows" and "Docker for Mac" have those same two pieces and will both run Linux Docker containers -- that's the point, after all -- but they rely on a small MobyLinux virtual machine to do so. This adds some performance overhead to your Docker containers (and requires you to allocate CPU cores, RAM, and disk space to your VM up front) but this is fine for development. You aren't meant to run Docker for Windows or Docker for Mac in a production environment.

**Windows 10 Pro & Enterprise or Windows Server 2008+: Enable Hyper-V**

Docker for Windows employs Hypervisor virtualization to run the MobyLinux VM. If you don't have Hyper-V support, or you are running Windows 7 or Windows 10 Home Edition, there are other options for running Docker \(either Virtualbox or Docker Toolbox\); please consult the [Docker for Windows Installation Guide](https://docs.docker.com/docker-for-windows/install/) for more information. This guide assumes you can install either Docker for Windows or Docker Toolbox. 

{% hint style="info" %}
**A Note on Docker for Windows and Windows Containers**

Docker for Windows gives you a choice of using Linux Containers or Windows Containers. Select **Linux Containers**. If your pipeline was entirely windows-based, you would use Windows containers, but as of August 2018 this is possible but not yet advisable. The Commandbox image upon which this guide relies is Linux-based, and the authors have not tried containerizing IIS. This process is likely to become easier over the next year or so as Windows 2019 promises to focus on containers, but this guide doesn't ye\) cover a full-scale Windows production deployment. The authors use windows for development, but we'll want to use the same flavor of container everywhere in our pipeline, and that's Linux containers.

**Windows Server 2016 & 2019**

Beginning with Windows Server 2016 and the Windows 10 Fall 2017 "Creators" update, Microsoft now natively supports Docker through Microsoft's "LCOW," or **L**inux **C**containers **O**n **W**indows on Docker 18.02 and later. This means that you can run "Docker" without the MobyLinux VM and don't need "Docker for Windows." For the time being, LCOW is still an experimental feature, and this guide will assume that Windows users are running Docker for Windows; but most everything should work just the same. As of the Spring of 2018, there were some issues with LCOW containers and hard memory limits that could interfere with some parts of this guide (particularly if you run SQL Server); but LCOW seems likely to become the standard for running Linux containers on Windows machines, whether in Development or Production.

{% endhint %}

### \(Docker for Windows & Mac Only\) Configure Docker Resources

The default resource consumption parameters for Docker containers are that they may use any and all CPU and memory resources available to them. On Linux \(or a native Windows Server 2016+\) Docker install, that's all of the resources on the host machine. On Docker for Windows or Docker for Mac, Docker runs inside a MobyLinux virtual machine, so containers may only make use of the resources you allocate to the VM in your Docker settings.

The default installation of Docker for Windows and Docker for Mac imposes significant limits on the MobyLinux VM. Since we will be running our CF engine, a database, a front-end web server, and \(optionally\) a cache server, we'll want to allocate additional RAM and CPU. 4 gigabytes of RAM and 1 CPU is probably sufficient, but it depends on how resource-hungry the applications you're developing are. If you have CPU cores or memory to spare, allocate as much as you'd like for your development environment. If you're running MSSQL Linux rather than MySQL, we recommend 6 GB rather than 4; our development machines at inLeague have at least 16 gigs of RAM and we'll allocate at least 8 to Docker.

Of course, if you're running "Docker native" (on Linux or Windows Server 2016+) you can skip this step as Docker doesn't require an up-front allocation of resources. 

![Figure 3.3: Sam&apos;s Docker for Windows Resource Settings](/.gitbook/assets/snip_20180501102036.png)

### Verify Your Docker Installation

In your favorite terminal, run:
```docker version```

If Docker is running correctly, you should see two results -- one for the engine (server) and one for the client (except your '**version**' will be 18.06.0 or higher as of August 2018)

![Figure 3.3: docker version](/assets/docker-version.png)

