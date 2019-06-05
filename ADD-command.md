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

#### Pattern Matching in Source Names 

Wildcards are used for creating patterns in source names.

Examples:

```
ADD myfile* /home/       #This would add all files starting with "myfile" - like myfile1.txt, myfile.py, myfilefinal.sh, etc.
```

```
ADD myfile?.txt /home/      #This would match only a single character after "myfile" - like myfile1.txt
```

Wildcard matching rules are in accordance with Go language, the one in which Docker tool was built.

#### Destination paths

Destination can be an absolute path or a path relative to WORKDIR.
We can specify a working directory for the image using WORKDIR. This working directory
















