# Docker Example: nc-listener using Dockerfile

This Dockerfile creates a container with `nc` installed. A standalone app `nc-listener` is loaded into the image when it is built and is the default program when the container is run.

```bash
$ docker image build . -t nc-listener
$ docker run -it nc-listener
```

Now, you should be able to use netcat on another host to connect to the container:

```bash
$ nc -v <container-ip> <port>
```
