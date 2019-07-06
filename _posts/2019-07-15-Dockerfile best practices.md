### Dockerfile best practices

There are a few recommended best practices to consider when authoring a Dockerfile, which are as follows:

- `First and foremost`, we need to consider that containers are meant to be ephemeral. By ephemeral,we mean that a container can be stopped and destroyed and a new one built and put in place with an absolute minimum of setup and configuration. That means that we should try hard to keep the time that is needed to initialize the application running inside the container at a minimum, as well as the time needed to terminate or clean up the application.
- The next best practice tells us that we should order the individual commands in the Dockerfile so that we leverage caching as much as possible. Building a layer of an image can take a considerable amount of time, sometimes many seconds or even minutes. While developing an application, we will have to build the container image for our application multiple times. We want to keep the build times at a minimum.

When we're rebuilding a previously built image, the only layers that are rebuilt are the ones that have changed, but if one layer needs to be rebuilt, all subsequent layers also need to be rebuilt. This is very important to remember. Consider the following example:

```
FROM node:9.4
RUN mkdir -p /app
WORKIR /app
COPY . /app
RUN npm install
CMD ["npm", "start"]
```

In this example, the npm install command on line five of the Dockerfile usually takes the longest. A classical Node.js application has many external dependencies, and those are all downloaded and installed in this step. This can take minutes until it is done. Therefore, we want to avoid running npm install each time we rebuild the image, but a developer changes their source code all the time during development of the application. That means that line four, the result of the COPYcommand, changes all the time and this layer has to be rebuilt each time. But as we discussed previously, that also means that all subsequent layers have to be rebuilt, which in this case includes the npm install command. To avoid this, we can slightly modify the Dockerfile and have the following:

```
FROM node:9.4
RUN mkdir -p /app
WORKIR /app
COPY package.json /app/
RUN npm install
COPY . /app
CMD ["npm", "start"]
```

What we have done here is that, on line four, we only copy the single file that the npm install command needs as a source, which is the package.json file. This file rarely changes in a typical development process. As a consequence, the npm install command also has to be executed only when the package.json file changes. All the remaining, frequently changed content is added to the image after the npm install command.

- `A further best practice` is to keep the number of layers that make up your image relatively small. The more layers an image has, the more the graph driver needs to work to consolidate the layers into a single root filesystem for the corresponding container. Of course, this takes time, and thus the fewer layers an image has, the faster the startup time for the container can be.

But *how can we keep our number of layers low?* Remember that in a Dockerfile, each line that starts with a keyword, such as FROM, COPY, or RUN, creates a new layer. The easiest way to reduce the number of layers is to combine multiple individual RUN commands into a single one—for example, say that we had the following in a Dockerfile:

```
RUN apt-get update
RUN apt-get install -y ca-certificates
RUN rm -rf /var/lib/apt/lists/*
```

We could combine these into a single concatenated expression, as follows:

```
RUN apt-get update \
    && apt-get install -y ca-certificates \
    && rm -rf /var/lib/apt/lists/*
```

The former will generate three layers in the resulting image, while the latter only creates a single layer.

The next three best practices all result in smaller images. `Why is this important? `Smaller images reduce the time and bandwidth needed to download the image from a registry. They also reduce the amount of disk space needed to store a copy locally on the Docker host and the memory needed to load the image. Finally, smaller images also means a smaller attack surface for hackers. Here are the best practices mentioned:

- The first best practice that helps to reduce the image size is to use a .dockerignore file. We want to avoid copying unnecessary files and folders into an image to keep it as lean as possible. A .dockerignore file works in exactly the same way as a .gitignore file, for those who are familiar with Git. In a .dockerignore file, we can configure patterns to exclude certain files or folders from being included in the context when building the image.
- The next best practice is to avoid installing unnecessary packages into the filesystem of the image. Once again, this is to keep the image as lean as possible.
- `Last but not least`, it is recommended that you use multistage builds so that the resulting image is as small as possible and only contains the absolute minimum needed to run your application or application service.