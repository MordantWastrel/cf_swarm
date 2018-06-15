---
description: 'Local Installation and initial configuration (Time Required: 5 Minutes)'
---

# Part 3: Docker in Development

## The Prize: Turnkey Development Environment

In this section, we'll install Docker, configure resource limits, and work with a **Docker Compose** file to configure containers for a database, a front-end web server, our Coldfusion application server, and \(optionally\) a dedicated cache.  By the end of this section, you should have a Docker environment ready to be deployed on any development machine, and that environment will look something like this:

![Figure 3.1: Local Development Topology ](/.gitbook/assets/cf-development-diagram.png)

## Get Docker

{% hint style="info" %}
Familiarity with Docker is not a pre-requisite for this guide; you can simply follow the instructions and you'll acquire a basic understanding of how Docker works just by seeing it in action. Nevertheless, if you are able to invest some time up front into learning about Docker generally, you will have an easier time in the long run -- particularly in the later, Production sections.

Our favorite Docker resources are:

* The [Udemy Docker Mastery course](https://udemy.com/docker-mastery/) by Bret Fisher -- it can be completed in a week or less and is one of the best and most accessible entry points for the Docker universe.
* The Ortus Container Roadshow from 2017, which covers containerizing CF more specifically
{% endhint %}

Start by installing [Docker CE](https://store.docker.com/search?type=edition&offering=community) \(Community Edition\) on your machine.  It doesn't matter whether you're using a Windows PC, a Mac, or Linux; get the version of Docker appropriate to your OS. For local development, we like to use the Edge channel, which is a monthly release that has the latest features; however, nothing in this guide requires those and the Stable channel is fine if you prefer.

{% hint style="info" %}
**Windows 10 Pro & Enterprise or Windows Server 2008+: Enable Hyper-V**

Docker for Windows employs Hypervisor virtualization to run a small MobyLinux virtual machine. If you don't have Hyper-V support, or you are running Windows 7 or Windows 10 Home Edition, there are other options for running Docker \(either Virtualbox or Docker Toolbox\); please consult the [Docker for Windows Installation Guide](https://docs.docker.com/docker-for-windows/install/) for more information. This guide assumes you can install either Docker for Windows or Docker Toolbox. 

**A Note on Docker for Windows and Windows Containers**

Docker for Windows gives you a choice of using Linux Containers or Windows Containers. Select **Linux Containers**. If your pipeline was entirely windows-based, you would use Windows containers, but as of May 2018 this is possible but not yet advisable. The Commandbox image upon which this guide relies is Linux-based, and the authors have not tried containerizing IIS. This process is likely to become easier over the next year or so as Windows 2019 promises to focus on containers, but this guide doesn't ye\) cover a full-scale Windows production deployment. The authors use windows for development, but we'll want to use the same flavor of container everywhere in our pipeline, and that's Linux containers.

**Windows Server 2016 & 2019**

Beginning with Windows Server 2016, Microsoft now natively supports Docker -- meaning you can run "Docker" and don't need "Docker for Windows," as the later runs a tiny MobyLinux VM with a small amount of performance overhead to make everything work on Windows. If you're running Windows Server 2016+ for local development, you can use vanilla Docker. 
{% endhint %}

### \(Docker for Windows & Mac Only\) Configure Docker Resources

The default resource consumption parameters for Docker containers are that they may use any and all CPU and memory resources available to them. On Linux \(or a native Windows Server 2016+\) Docker install, that's all of the resources on the host machine. On Docker for Windows or Docker for Mac, Docker runs inside a MobyLinux virtual machine, so containers may only make use of the resources you allocate to the VM in your Docker settings.

The default installation of Docker for Windows and Docker for Mac runs a  imposes significant limits on the MobyLinux VM. Since we will be running our CF engine, a database, a front-end web server, and \(optionally\) a cache server, we'll want to allocate additional RAM and CPU. 4 gigabytes of RAM and 1 CPU is probably sufficient, but it depends on how resource-hungry the applications you're developing are. If you have CPU cores or memory to spare, allocate as much as you'd like for your development environment.

![Figure 3.2: Sam&apos;s Docker for Windows Resource Settings](/.gitbook/assets/snip_20180501102036.png)

