## Building Your Docker Images

{% hint style='tip' %}
Setting up a Build Environment and Building the Commandbox Image (Time Required: 10 Minutes)
{% endhint %}

The lion's share of the work in creating the Docker images used in the CF world has already been done, and we could spin up a CF container using "official" images -- whether Adobe's, Lucee's, or Commandbox -- without making any changes (especially since we can make a number of changes without altering the image at all, as we'll discover shortly). 

Since we're laying out infrastructure for an entire development pipeline, though, we'll want to make our own images, even if the changes we make might just be one or two lines to copy a configuration file. So before we get to our development environment, let's put in some plumbing that will let us easily build the Docker images we'll deploy in dev later.