# About the Author

[Samuel Knowlton](mailto:sam@inleague.org) is the founder and lead developer of inLeague LLC, a boutique application development shop founded in 2002 and located since 2012 in Austin, Texas. He holds a BA in English Literature from Cornell University. When he isn't writing code or enjoying cats, motorcycles, or airplanes, he can be found on stage at the [Austin Playhouse.](http://www.austinplayhouse.com)

# License

You are free to use, share, and adapt \(with attribution\) this guide according to the terms of the [Creative Commons Attribution-ShareAlike 4.0 International license.](https://creativecommons.org/licenses/by-sa/4.0/)

# Who is This Guide For?

This guide is for you if:

* You manage or maintain Coldfusion applications \(pr you want to\)
* You've heard of "containers" and perhaps Docker but you don't know a lot about them and what they can do for you
* You want to modernize your infrastructure and you understand the principles of containerization, but aren't sure where to begin

The guide assumes an intermediate level of familiarity with Coldfusion and server administration, but even if you're a beginner, we'll provide links to guides for topics we will mention but not cover in depth, like firewall administration or SSH key generation. 

# Why This Guide? or: Stop Adopting Pets

One of the larger investments of time and resources at our company has always been our service infrastructure model, which persisted as a bunch of servers in a data center for over ten years even as AWS and other cloud solutions became the standard. We liked our hosting company; they'd always given us a good deal and we even looked forward to naming new ones every so often. We're also a small company and for us to invest in new architecture might take half or three quarters of our manpower away from development.

At the [Coldfusion Summit](https://twitter.com/hashtag/CFSummit2017?src=hash) in November of 2017, at the end of a presentation given by Luis Majano \([@lmajano](https://twitter.com/lmajano)\)  at [Ortus Solutions](https://www.ortussolutions.com/) on an unrelated subject, he had a few minutes left and decided to containerize and deploy the demo application he'd written. He did so with time to spare, and we realized that we were past due in adopting this deployment strategy for our own applications. It wasn't that what we had suddenly became insufficient; rather, it became so easy to "do it right" that maintaining our old infrastructure would have been a larger and more restrictive investment than starting from scratch. It was time to retire the pets and hire some cattle.

A previous presentation had included the below slide, originally from [@randybias](https://www.slideshare.net/randybias/the-cloud-revolution-cyber-press-forum-philippines) at Cloudscaling:

![](/assets/servers_pets_or_cattle.jpg)

## Steal What Works

We began by looking for existing resources on cloud Coldfusion deployments. We attended no fewer than four sessions on the subject at CF Summit and found a range of strategies; some were appealing and others had time-consuming shortcomings.  All of them assumed you already had cloud architecture provisioned and focused primarily on getting Coldfusion into container form. We wanted to use the opportunity to re-think our entire workflow, using best practices from the CF community.

The foundation for this effort was the excellent [Docker Roadshow](https://www.ortussolutions.com/events/container-roadshow-2017) hosted by Ortus in September of 2017. Their work is the basis for this guide \(along with indispensable utilities like [Commandbox](https://www.ortussolutions.com/products/commandbox) and [CFConfig](https://www.forgebox.io/view/commandbox-cfconfig)\).  See Credits & Acknowledgments for the full list!

# Why "End to End?"

About forty miles west of us in Austin, Texas, there is a [bourbon distillery](http://www.garrisonbros.com/) where nearly the entire process takes place on the premises: they mix the mash, distill it, barrel it, age it, and finally bottle it all right there. Being able to see how everything works together makes it easier to understand every component, particularly if you go in not knowing anything about bourbon.  That's what we're trying to accomplish with this guide: provide enough detail so that you can reproduce and use the strategies we like while maintaining a sense of perspective on where each stage fits in the big picture.

There is already a great deal of information about Docker and Coldfusion on the 'net, and Adobe is working on an "official" CF Docker image. This guide borrows liberally from existing work \(with attribution\) but we don't have any religion about our choices: when possible, we've provided alternatives to our choices, and we're happy to hear from you if you think we've missed something.

# What You'll Have When You're Finished

* A development environment you can use on Windows, MacOS, or Linux, and easily replicate for others
* 2 or more cloud-hosted virtual machines at one of several competing infrastructure providers \(one for your database and one for your Docker containers\)
* Docker images for CF development, staging, and production
* \(Optional\) A Docker image for a distributed cache
* An understanding of how to scale your infrastructure when the need arises, both vertically \(feeding CF through embiggening your VM\) and horizontally \(CF clustering, Docker Swarm, and Distributed Caching\)



