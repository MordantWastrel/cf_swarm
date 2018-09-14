# Part 7: Container Strategy

While most of this guide is concerned with the preparation your application servers for containerization, we're going to need some tools to manage those containers and how they work together on our network. Even if we only have one or two containers to start with, part of why we're making this investment is to enjoy the benefits of easy scalability. Also, even if we only end up running a single container for our CF engine, we still want to be able to do "rolling updates" of that container, which leads us to the concept of **orchestration:** a standardization of container environment beyond the specific hardware host and operating system upon which the containers are running.

As of April 2018, the two options available to us are [Docker Swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/) \(maintained by Docker\) and [Kubernetes](https://kubernetes.io/) \(A very large FOSS project\).

{% hint style="info" %}
### Aside: More on Container Orchestration

Matthew Revell has [an excellent introductory guide](https://www.exoscale.com/syslog/container-orch/) to the concept of container orchestration. It was written in 2016, and some of the shortcomings mentioned in the specific sections on both Docker and Kubernetes have been addressed if not solved outright, though his characterization is still on point.
{% endhint %}

## Docker Swarm: Accessible and Suitable for Small-to-Medium

Since we're aiming for a small investment of time and a solution appropriate for small-to-medium enterprise applications, we'll be using [Docker Swarm](https://docs.docker.com/engine/swarm/swarm-tutorial/) with the lightweight management GUI [Portainer](https://portainer.io) to manage our containers.

{% hint style="info" %}
### Aside: Kubernetes Is Possibly the Future

There is considerable discussion within the containerization community regarding the future of orchestration, and many of the biggest players have lined up behind Kubernetes. While the industry has largely accepted Docker as a container _format_ standard, there is some resistance to relying on a single company for a container _management_ standard, especially when the alternative has support from much larger organizations like Google and IBM.

We spent several days investigating Kubernetes as a solution for this guide, and while it was very promising and exciting, it has a much steeper learning curve in part because it provides features and tools suited to all kinds of scenarios that we're unlikely to encounter in this guide.
{% endhint %}

Any guide attempting to describe an end-to-end software pipeline will rely on all kinds of assumptions and generalizations; ordinarily, we try to let you know through these asides when we're making any when discussing subjects with many solutions that depend on your particular need \(for instance, Firewall or VPN topology\). Nowhere is this more the case then with container orchestration. Docker Swarm is the only tool that let us get off the ground quickly enough to be useful for this guide, but we expect that we'll have to revisit this section once Kubernetes matures to a point where it's suitable "out of the box" for a simple pipeline such as ours without a lot of setup time.

