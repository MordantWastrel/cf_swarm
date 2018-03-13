# What Containers Mean to Web Applications \(and You\)

## Hypervisors, Dockers, and Bears!

If you're reading this guide, you may already have some idea of the principles behind cloud hosting and containerized deployment. If these things still sound like buzzwords rather than concrete concepts, then it's a good idea to start with a brief introduction, like [A Beginner-Friendly Introduction to Containers, VMs and Docker](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b) by Preethi Kasireddy.

For the purposes of this guide, "Containers" means Docker Images, but Docker is not the only container solution in use. Google's Kubernetes is growing in popularity, but it's not quite an apples-to-apples comparison \(for example, you can run Docker in Kubernetes\). Future versions of this guide may cover Docker-alternatives, but as of this writing, Docker has broad community support and best fits our "...in a day" mission.

While the bulk of the guide is dedicated to deploying a Coldfusion pipeline, the governing principles are the same regardless of the language -- that's the whole point!

## Nuts and Bolts for a CF Developer

The most important concept is represented by the [Pets vs. Cattle](/README.md) cartoon in the Preface. Historically, Your application server \(and your front-end web server\) should be **replicable** and **undistinguished.  **That's not to say that it can't  **customized** for your applications; obviously, you'll have specific Application and CF Admin settings, but you'll configure those customizations to apply automatically every time you start a new server.

##### FAQ: I just need a place to run my application! Why should I bother with all this?

Using containers is easier, faster, and cheaper in the long run. If you're accustomed to provisioning servers and anointing this or that server as "Production" versus "Staging" and then maintaining those servers, the biggest obstacle to an easier job is simply familiarity, and that's what this guide is here to provide.

In our shop, prior to our container/cloud migration, we'd been with the same hosting company for over ten years and we had a great relationship with them. They aggressively discounted their services and always responded to tickets promptly and courteously -- so much so that we held off exploring a container strategy simply because what we had "just worked" and all of our practices and training were centered on managing the pipeline that had served us well for so long. It took a lot to make us leave; the business case for containers had to be very strong.   It is.

#### Cloud, Container, and Distributed: Different Concepts but Complementary

##### FAQ: I don't need lots of Coldfusion servers. Just one! Isn't this "distributed cache" and "swarm" stuff for other people?

If you're new to Docker, one area of confusion might be the common association between Docker, Cloud deployments, and "Distributed" or "Clustered" applications. The fact is that **you can use one or all of these concepts as part of your strategy without committing to all of them**. 

Want to learn Docker and move away from your "metal box" server but intimidated by \(or totally unaware of\) the idea of session clusters? No problem. Just interested in moving to scalable, cloud-hosted VMs but not yet ready to work with Docker? Everything in [Part 3: Choosing a Cloud Infrastructure](/part-2-choosing-a-cloud-infrastructure-provider.md) applies to you, and you can come back later for the rest of the guide.

These terms come up in tandem because they enable one another, but not because you have to do all of them. You can deploy a single Docker image with your CF engine and run all the applications you like on it. Rather, once you've made the investment in cloud infrastructure and have your applications Docker-ready, the marginal cost of making them distributed -- that is, running the same application on multiple containers -- is manageable, and the payoff is considerable. 

However, preparing an existing CF application for Docker deployment is a different task than preparing it for distributed \(clustered\) deployment. We'll cover the specifics later and tackle one task at a time.

#### Replicable and Undistinguished ... but Customized

These terms reflect a design philosophy governing your application development that is an extension of the "Cattle" metaphor. We'll see what they mean in-depth later on, but we can summarize the philosophy for now: 

* **Replicable**: If you spin up a Docker Image with your Coldfusion app, then shut it down and spin up another one, they should be identical: no manual effort \(like editing a file or downloading a library\) can be required in between starting the Image and your application working correctly.

* **Undistinguished: **Another aspect of "replicable" is that nothing should distinguish one container running your application from another. For example, your application cannot save anything other than temporary files to its local file system. If your app has a document storage module and currently saves locally, you'll need to tweak it to use something like [Block Storage](https://www.digitalocean.com/products/storage/) or else Amazon S3, because you have to presume that the filesystem local to the Image will be destroyed.

* **Customized: **Neither "replicable" nor "undistinguished" preclude even a high level of customization, whether of your Coldfusion engine, JRE, or front-end web server.  Some of these are easy \(like Commandbox and CF Admin/CFConfig settings\) while others require a little more legwork using Docker utilities, but you'll be able to set environment variables, configure your web server, and your CF engine however you need -- and then spend a lot less time worrying about those configurations later since you'll have all of them in one place!



