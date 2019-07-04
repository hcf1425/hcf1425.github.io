### The WORKDIR keyword

The WORKDIR keyword defines the working directory or context that is used when a container is run from our custom image. So, if I want to set the context to the /app/bin folder inside the image, my expression in the Dockerfile would have to look as follows:

```
WORKDIR /app/bin
```

All activity that happens inside the image after the preceding line will use this directory as the working directory. It is very important to note that the following two snippets from a Dockerfile are not the same:

```
RUN cd /app/bin
RUN touch sample.txt
```

Compare the preceding code with the following code:

```
WORKDIR /app/bin
RUN touch sample.txt
```

**The former will create the file in the root of the image filesystem, while the latter will create the file at the expected location in the /app/bin folder. Only the WORKDIR keyword sets the context across the layers of the image. The cd command alone is not persisted across layers.**

