### 3. Saving and  loading images

The third way to create a new container image is by importing or loading it from a file. A container image is nothing more than a tarball. To demonstrate this, we can use the docker image save command to export an existing image to a tarball:

```
$ docker image save -o ./backup/my-alpine.tar my-alpine
```

The preceding command takes our my-alpine image that we previously built and exports it into a ./backup/my-alpine.tar file.

If, on the other hand, we have an existing tarball and want to import it as an image into our system, we can use the docker image load command as follows:

```
$ docker image load -i ./backup/my-alpine.tar
```

