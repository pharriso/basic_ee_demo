Disconnected ansible-builder example
=========

This directory contains an example ansible-builder configuration. I didn't want to have to edit the ansible-builder image so I'm loading things in as part of the Containerfile.

I used nexus as a python proxy in my lab to test this.

Steps
--------------

This example repo contains the Containerfile as an example. To generate this and customise it:


Clear out old context/Containerfile and context/_build

Run ansible-builder to generate the Containerfile

```bash
ansible-builder create
```

Edit the resulting Container file. In my example I made the following edits for both the EE base image and EE builder image:

```bash
# Remove UBI repo, add my pip.conf and ca cert
RUN rm -f /etc/yum.repos.d/ubi.repo
ADD pip.conf /etc/pip.conf
ADD demolab-ca.crt /etc/pki/ca-trust/source/anchors/demolab-ca.crt
RUN update-ca-trust
# end of modifications
```

Copy files into context directory
--------------

Copy any config files you need into the context directory. In my case this included:

* pip.conf - pointing to my nexus repo for python dependencies
* ca cert so I can talk to my nexus repo. In this example I didn't actually put the CA cert into github

Building the execution environment
--------------

To build the execution environment run:

```bash
podman build -f context/Containerfile -t my_registry.example.com/my_ee:1.0
```
