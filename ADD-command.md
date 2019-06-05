# Injecting Files into your Image using ADD

It is the usual requirement of a developer to copy a few files into the docker image so that every container spawned from this image has the required files.

Docker allows us to do this using two isntructions in a Dockerfile: 1) ADD 2) COPY

## ADD instruction

This lets us copy our files/directories from a source (lying on the local filesystem of base system or at a remote site) to the destination filesystem of the image.
For copying files present at some remote site, provide URL in source field. Multiple sources can be specified.

ADD instruction takes the following format:

      ADD [--chown=user:group] (source1) (source2) ... (destination)
OR

      ADD [--chown=user:group] ("source"), ("source2"), ... ("destination")

The second form is preferred when paths contain whitespaces.

The option 'chown' lets us control the user and group for the destination filesystem. This option is applicable only to Linux containers.

If the destination path doesn't exist on the base image, it is created along with all parent directories.

Usage:

```
ADD hello.py /root/home/hello.py
```

