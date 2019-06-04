# What is a Dockerfile?
A Dockerfile is a text file which contains a series of commands or instructions. These instructions are executed in the order in which they are written.
Execution of these instructions takes place on a base image. Hence, on building the Dockerfile, the successive actions form a new image from the base parent image.

To write a Dockerfile, we need to keep in mind the following keywords:

### FROM
This directive lets a user define a base image to initiate the build process. Any image can be provided here - including the ones a user creates by himself.
If the image specified is not found on the host, Docker attempts to search and download it from Docker Hub (public repository) or any other specified private repositary.

Usage: FROM image-name
```
FROM centos:latest
```

### ADD
This command takes two arguments -  a source and a destination. The files present on the source (may be localhost system or some remote URL) are copied to the destination specified path in the container file system. 

Usage: ADD (source directory from base system or URL) (destination filesystem of container)

```
ADD /hello /hello
```

### VOLUME
This command lets user mount a volume or directory present on base system on the container. Thus, it is like enabling access to the base's directory which is mounted and any changes made to it are visible to the container.

```
VOLUME [/a1 /a2 /a3]
```

### MAINTAINER
It is used to provide author's name to the image formed.

```
MAINTAINER Prashansa Kulshrestha
```

### ENV
It is used to specify environment variables in the form of key-value pairs.

```
ENV DISPLAY :0
```
### LABEL
It is used to provide metadata for an image and is provided as key-value pair.

```
LABEL version="1.0"
```
### RUN
This takes a command as an argument and runs it over the base image to form a new layer. 


Default shell used for a Linux container is /bin/sh. For a Windows container, cmd /s /c shell is used by default.

```
RUN yum install httpd
```

### CMD
The shell command specified with CMD becomes the default command which executes when a container initiates from an image.
If used in conjunction with ENTRYPOINT command, CMD provides default argument to the ENTRYPOINT command.

```
CMD echo 'Hello World'
```

### ENTRYPOINT
It sets the concrete default application that is used every time a container is created from an image. It configures a container that can run as an executable.


