### Pushing images to a registry

Creating custom images is all well and good, but at some point, we want to actually share or ship our images to a target environment, such as a test, QA, or production system. For this, we typically use a container registry. One of the most popular and public registries out there is Docker Hub. It is configured as a default registry in your Docker environment, and it is the registry from which we have pulled all our images so far.

On a registry, one can usually create personal or organizational accounts. For example, my personal account at Docker Hub is gnschenker. Personal accounts are good for personal use. If we want to use the registry professionally, then we probably want to create an organizational account, such as acme, on Docker Hub. The advantage of the latter is that organizations can have multiple teams. Teams can have differing permissions.

To be able to push an image to my personal account on Docker Hub, I need to tag it accordingly. Let's say I want to push the latest version of alpine to my account and give it a tag of 1.0. I can do this in the following way:

```
$ docker image tag alpine:latest gnschenker/alpine:1.0
```

Now, to be able to push the image, I have to log in to my account:

```
$ docker login -u gnschenker -p <my secret password>
```

After a successful login, I can then push the image:

```
$ docker image push gnschenker/alpine:1.0
```

I will see something similar to this in the terminal:

```
The push refers to repository [docker.io/gnschenker/alpine]
04a094fe844e: Mounted from library/alpine
1.0: digest: sha256:5cb04fce... size: 528
```

For each image that we push to Docker Hub, we automatically create a repository. A repository can be private or public. Everyone can pull an image from a public repository. From a private repository, one can only pull an image if one is logged in to the registry and has the necessary permissions configured.

