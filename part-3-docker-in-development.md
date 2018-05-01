---
description: Getting Started with Docker for Local Development
---

# Part 3: Docker in Development

## Get Docker

{% hint style="info" %}
Familiarity with Docker is not a pre-requisite for this guide; you can simply follow the instructions and you'll acquire a basic understanding of how Docker works just by seeing it in action. Nevertheless, if you are able to invest some time up front into learning about Docker generally, you will have an easier time in the long run -- particularly in the later, Production sections.

Our favorite Docker resources are:

* The [Udemy Docker Mastery course](https://udemy.com/docker-mastery/) by Bret Fisher -- it can be completed in a week or less and is one of the best and most accessible entry points for the Docker universe.
* The Ortus Container Roadshow from 2017, which covers containerizing CF more specifically
{% endhint %}

Start by installing [Docker CE](https://store.docker.com/search?type=edition&offering=community) \(Community Edition\) on your machine.  It doesn't matter whether you're using a Windows PC, a Mac, or Linux; just get the version of Docker appropriate to your OS. For local development, we like to use the Edge channel, which is a monthly release that has the latest features, but nothing in this guide requires those and the Stable channel is fine if you prefer.

{% hint style="info" %}
**A Note on Docker for Windows and Windows Containers**

Docker for Windows will give you a choice of using Linux Containers or Windows Containers. Select **Linux Containers**. This guide doesn't \(yet\) cover a full-scale Windows production deployment. Windows is fine for development, but we'll want to use the same flavor of container everywhere in our pipeline, and that's Linux containers.

**Windows Server 2016 & 2019**

Beginning with Windows Server 2016, Microsoft now natively supports Docker -- meaning you can run "Docker" and don't need "Docker for Windows," as the later runs a tiny MobyLinux VM with a small amount of performance overhead to make everything work on Windows. If you're running Windows Server 2016+ for local development, you can use vanilla Docker. 
{% endhint %}



