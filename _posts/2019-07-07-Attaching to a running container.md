### Attaching to a running container

We can use the attach command to attach our Terminal's standard input, output, and error (or any combination of the three) to a running container using the ID or name of the container. Let's do this for our quotes container:

```
$ docker container attach quotes 
```

In this case, we will see every five seconds or so a new quote appearing in the output.

To quit the container without stopping or killing it, we can press the key combination *Ctrl*+*P* *Ctrl*+*Q*. This detaches us from the container while leaving it running in the background. On the other hand, if we want to detach and stop the container at the same time, we can just press *Ctrl*+*C*.

Let's run another container, this time an Nginx web server:

```
$ docker run -d --name nginx -p 8080:80 nginx:alpine 
```

Here, we run the Alpine version of Nginx as a daemon in a container named nginx. The -p 8080:80 command-line parameter opens port 8080 on the host for access to the Nginx web server running inside the container. Don't worry about the syntax here as we will explain this feature in more detail in the [Chapter 7](https://learning.oreilly.com/library/view/learn-docker-/9781788997027/ffbf091e-49db-4a86-805c-4476ffcc69d9.xhtml), *Single-Host Networking*.

Let's see whether we can access Nginx, using the curl tool and running this command:

```
$ curl -4 localhost:8080 
```

If all works correctly, you should be greeted by the welcome page of Nginx:

```
<html> 
<head> 
<title>Welcome to nginx!</title> 
<style> 
    body { 
        width: 35em; 
        margin: 0 auto; 
        font-family: Tahoma, Verdana, Arial, sans-serif; 
    } 
</style> 
</head> 
<body> 
<h1>Welcome to nginx!</h1> 
<p>If you see this page, the nginx web server is successfully installed and 
working. Further configuration is required.</p> 
 
<p>For online documentation and support please refer to 
<a href="http://nginx.org/">nginx.org</a>.<br/> 
Commercial support is available at 
<a href="http://nginx.com/">nginx.com</a>.</p> 
 
<p><em>Thank you for using nginx.</em></p> 
</body> 
</html> 
```

Now, let's attach our Terminal to the nginx container to observe what's happening:

```
$ docker container attach nginx 
```

Once you are attached to the container, you first will not see anything. But now open another Terminal, and in this new Terminal window, repeat the curl command a few times, for example, using the following script:

```
$ for n in {1..10}; do curl -4 localhost:8080; done  
```

You should see the logging output of Nginx, which looks similar to this:

```
172.17.0.1 - - [06/Jan/2018:12:20:00 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.54.0" "-"
172.17.0.1 - - [06/Jan/2018:12:20:03 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.54.0" "-"
172.17.0.1 - - [06/Jan/2018:12:20:05 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.54.0" "-"
...
```

Quit the container by pressing *Ctrl*+*C*. This will detach your Terminal and, at the same time, stop the nginx container.

To clean up, remove the nginx container with the following command:

```
$ docker container rm nginx 
```