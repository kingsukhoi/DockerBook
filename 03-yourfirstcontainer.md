# How to make a container

## Make your first container

Copy and paste the following code snippet into new file called `Dockerfile`.

Full dockerfile reference at [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

```text
FROM docker.io/fedora:33

RUN dnf install -y \ 
    nginx

COPY ./text.txt /copy/example/

ENTRYPOINT nginx -g 'daemon off;'
```

Here is a break down of each line

* `FROM docker.io/nginx:1.19.7`: This tells `docker build` to go to the docker.io registry, and find an image called "fedora". Docker.io is currently the most popular registry, but other services like GitLab provide the same service as well. The `:33` means pull an image with the tag "33". Docker uses the concept of tags to figure out which image you want. **WARNING,** tags can change image. By default, docker will pull the tag 'latest'. 
* `RUN dnf install -y nginx`: For Dockerfiles, the first word of each line is a command to `docker build`. Note, it's best practice for the first word to be in all caps.  


  The `RUN` command tells docker to run the next line, in the shell. If the command results in a change Docker will store the change and move onto the next line. If the command fails, the entire build will fail.  
 

  A backslash and a new line will tell docker to escape the newline, and inclue it in the current line. This is useful if you have a massive command. 

* `COPY ./test.txt /copy/example/`: This will copy a file called `./test.txt` to a folder called /copy/example/. If that folder doesn't exist in the container, it'll create it.
* `ENTRYPOINT nginx -g 'daemon off;'`: The `ENTRYPOINT` command tells docker to run the command `nginx -g 'daemon off;'` command every time the container starts up.

## Build the container

To build this container, create a file called `text.txt`, and create a file called `Dockerfile` and add the above code into it. Open a terminal in the same place as your Dockerfile and run `docker build -t first-container .`.   
 By default, Docker looks for a file called `Dockerfile` in the specified directory. The period at the end of the command is terminal shorthand for the current directory.

## Run the container

In the terminal, run `docker run -p 8080:80 first-container`. This tells docker to:

* Start `first-container`
* bind port 8080 on the host computer \(where you live\) to port 80 in the container. 

