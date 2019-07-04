# Anatomy of containers

Many individuals wrongly compare containers to VMs. However, this is a questionable comparison. Containers are not just lightweight VMs. OK then, *what is the correct description of a container?*

**Containers are specially encapsulated and secured processes running on the host system.**

Containers leverage a lot of features and primitives available in the Linux OS. The most important ones are **namespaces** and **cgroups**. `All processes running in containers share the same Linux kernel of the underlying host operating system. `This is fundamentally different compared with VMs, as each VM contains its own full-blown operating system.

`The startup times of a typical container can be measured in milliseconds, while a VM normally needs several seconds to minutes to startup.` VMs are meant to be long-living. It is a primary goal of each operations engineer to maximize the uptime of their VMs. Contrary to that, containers are meant to be ephemeral. They come and go in a quick cadence.

Let's first get a high-level overview of the architecture that enables us to run containers.