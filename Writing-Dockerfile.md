# What is a Dockerfile?
A Dockerfile is a text file which contains a series of commands or instructions. These instructions are executed in the order in which they are written.
Execution of these instructions takes place on a base image. Hence, on building the Dockerfile, the successive actions form a new image from the base parent image.

To write a Dockerfile, we need to keep in mind the following keywords:

### FROM
This directive lets a user define a base image to initiate the build process. Any image can be provided here - including the ones a user creates by himself.
If the image specified is not found on the host, Docker attempts to search and download it from Docker Hub (public repositary) or any other specified private repositary.

Usage: FROM image-name
```
FROM centos:latest
```

### ADD
This command takes two arguments -  a source and a destination. The files present on the source (may be localhost system or some remote URL) are copied to the destination system.
