# Stop Adopting Pets, or: Why "End to End?"

One of the larger investments of time and resources at our company has always been our service infrastructure model, which persisted as a bunch of servers in a data center for over ten years even as AWS and other cloud solutions became the standard. We liked our hosting company; they'd always given us a good deal and we even looked forward to naming new ones every so often. We're also a small company and for us to invest in new architecture might take half or three quarters of our manpower away from development. 

At the [Coldfusion Summit](https://twitter.com/hashtag/CFSummit2017?src=hash) in November of 2017, at the end of a presentation given by Luis Majano \([@lmajano](https://twitter.com/lmajano)\)  at [Ortus Solutions](https://www.ortussolutions.com/) on an unrelated subject, he had a few minutes left and decided to containerize and deploy the demo application he'd written. He did so with time to spare, and we realized that we were past due in adopting this deployment strategy for our own applications.

A previous presentation had included the below slide, originally from [@randybias](https://www.slideshare.net/randybias/the-cloud-revolution-cyber-press-forum-philippines) at Cloudscaling:

![](blob:https://www.gitbook.com/c702f400-4398-4c9d-90c5-44c8704717b4)

  


Our first rule of design is "Steal What Works," and so we started by looking for existing resources on cloud Coldfusion deployments. We attended no fewer than four sessions on the subject at CF Summit and found a range of strategies; some were appealing and others had time-consuming shortcomings. None of them used 

with the excellent [Docker Roadshow](https://www.ortussolutions.com/events/container-roadshow-2017) hosted by Ortus in September of 2017. Their work is the basis for this guide \(along with indispensable utilities like [Commandbox](https://www.ortussolutions.com/products/commandbox) and [CFConfig](https://www.forgebox.io/view/commandbox-cfconfig)\).



