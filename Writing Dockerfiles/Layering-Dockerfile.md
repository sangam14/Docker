# Layering Concept Explained with Dockerfile

Docker container is a runnable instance of an image, which is actually made by writing a readable/writable layer on top of some read-only layers. 

The parent image used to create another image from a Dockerfile is read-only. When we execute instructions on this parent image, new layers keep adding up.
These layers are created when we run docker build command. 

The instructions RUN, COPY, ADD mostly contribute to the addition of layers in a Docker build. 

Each layer is read-only except the last one - this is added to the image for generating a runnable container. This last layer is called "container layer". All changes made to the container, like making new files, installing applications, etc. are done in this thin layer.

Let's understand this layering using an example:

Consider the Dockerfile given below:

![Docker layers in Dockerfile](https://github.com/Prashansa-K/Docker/blob/master/Writing%20Dockerfiles/layering1.png)

### Layer ID
Each instruction the Dockerfile generates a layer. Each of this layer has a randomly generated unique ID. This ID can be seen at the time of build. See the image below:

![Docker layers during Build](https://github.com/Prashansa-K/Docker/blob/master/Writing%20Dockerfiles/layering2.png)

To view all these layers once an image is built from a Dockerfile, we can use docker history command.

![Docker history]()
