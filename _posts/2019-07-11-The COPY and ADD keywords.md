### The COPY and ADD keywords

The COPY and ADD keywords are very important since, in the end, we want to add some content to an existing base image to make it a custom image. Most of the time, these are a few source files of, say, a web application or a few binaries of a compiled application.

These two keywords are used to copy files and folders from the host into the image that we're building. The two keywords are very similar, `with the exception that the ADD keyword also lets us copy and unpack TAR files, as well as provide a URL as a source for the files and folders to copy.`

Let's look at a few examples of how these two keywords can be used:

```
COPY . /app
COPY ./web /app/web
COPY sample.txt /data/my-sample.txt
ADD sample.tar /app/bin/
ADD http://example.com/sample.txt /data/
```

In the preceding lines of code:

- The first line copies all files and folders from the current directory recursively to the /app folder inside the container image
- The second line copies everything in the web subfolder to the target folder, /app/web
- The third line copies a single file, sample.txt, into the target folder, /data, and at the same time, renames it to my-sample.txt
- The fourth statement unpacks the sample.tar file into the target folder, /app/bin
- Finally, the last statement copies the remote file, sample.txt, into the target file, /data

Wildcards are allowed in the source path. For example, the following statement copies all files starting with sample to the mydir folder inside the image:

```
COPY ./sample* /mydir/
```

From a security perspective, it is important to know that by default, all files and folders inside the image will have a **user ID** (**UID**) and a **group ID** (**GID**) of 0. The good thing is that for both ADD and COPY, we can change the ownership that the files will have inside the image using the optional --chown flag, as follows:

```
ADD --chown=11:22 ./data/files* /app/data/
```

The preceding statement will copy all files starting with the name web and put them into the /app/data folder in the image, and at the same time assign user 11 and group 22 to these files.

Instead of numbers, one could also use names for the user and group, but then these entities would have to be already defined in the root filesystem of the image at /etc/passwd and /etc/group respectively, otherwise the build of the image would fail.