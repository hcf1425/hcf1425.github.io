### The CMD and ENTRYPOINT keywords

The CMD and ENTRYPOINT keywords are special. While all other keywords defined for a Dockerfile are executed at the time the image is built by the Docker builder, these two are actually definitions of what will happen when a container is started from the image we define. When the container runtime starts a container, it needs to know what the process or application will be that has to run inside this container. That is exactly what CMD and ENTRYPOINT are used for—to tell Docker what the start process is and how to start that process.

Now, the differences between CMD and ENTRYPOINT are subtle, and honestly most users don't fully understand them or use them in the intended way. Luckily, in most cases, this is not a problem and the container will run anyway; it's just the handling of it that is not as straightforward as it could be.

To better understand how to use the two keywords, let's analyze what a typical Linux command or expression looks like—for example, let's take the ping utility as an example, as follows:

```
$ ping 8.8.8.8 -c 3
```

In the preceding expression, ping is the command and 8.8.8.8 -c 3 are the parameters to this command. Let's look at another expression:

```
$ wget -O - http://example.com/downloads/script.sh
```

Again, in the preceding expression, wget is the command and -O - http://example.com/downloads/script.sh are the parameters.

Now that we have dealt with this, we can get back to CMD and ENTRYPOINT. ENTRYPOINT is used to define the command of the expression while CMD is used to define the parameters for the command. Thus, a Dockerfile using alpine as the base image and defining ping as the process to run in the container could look as follows:

```
FROM alpine:latest
ENTRYPOINT ["ping"]
CMD ["8.8.8.8", "-c", "3"]
```

For both ENTRYPOINT and CMD, the values are formatted as a JSON array of strings, where the individual items correspond to the tokens of the expression that are separated by whitespace. This the preferred way of defining CMD and ENTRYPOINT. It is also called the **exec** form.

Alternatively, one can also use what's called the **shell** form, for example:

```
CMD command param1 param2
```

We can now build an image from the preceding Dockerfile, as follows:

```
$ docker image build -t pinger .
```

Then, we can run a container from the pinger image we just created:

```
$ docker container run --rm -it pinger
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=37 time=19.298 ms
64 bytes from 8.8.8.8: seq=1 ttl=37 time=27.890 ms
64 bytes from 8.8.8.8: seq=2 ttl=37 time=30.702 ms
```

The beauty of this is that I can now override the CMD part that I have defined in the Dockerfile (remember, it was ["8.8.8.8", "-c", "3"]) when I create a new container by adding the new values at the end of the docker container run expression:

```
$ docker container run --rm -it pinger -w 5 127.0.0.1
```

This will now cause the container to ping the loopback for 5 seconds.

If we want to override what's defined in the ENTRYPOINT in the Dockerfile, we need to use the --entrypointparameter in the docker container run expression. Let's say we want to execute a shell in the container instead of the ping command. We could do so by using the following command:

```
$ docker container run --rm -it --entrypoint /bin/sh pinger
```

We will then find ourselves inside the container. Type exit to leave the container.

As I already mentioned, we do not necessarily have to follow best practices and define the command through ENTRYPOINT and the parameters through CMD, but we can instead enter the whole expression as a value of CMD and it will work:

```
FROM alpine:latest
CMD wget -O - http://www.google.com
```

Here, I have even used the **shell** form to define the CMD. But what does really happen in this situation where ENTRYPOINT is undefined? If you leave ENTRYPOINT undefined, then it will have the default value of /bin/sh -c, and whatever is the value of CMD will be passed as a string to the shell command. The preceding definition would thereby result in entering following process to run inside the container:

```
/bin/sh -c "wget -O - http://www.google.com"
```

**Consequently, /bin/sh is the main process running inside the container, and it will start a new child process to run the wget utility.**

