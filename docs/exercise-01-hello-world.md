\# Exercise 01: Hello World



\## Objective



Build a simple Docker image using Ubuntu 20.04 as its base image and execute a command inside a container.



\## Dockerfile Explanation



```dockerfile

FROM ubuntu:20.04

CMD \["echo", "Hello World"]

```



\* `FROM ubuntu:20.04`: Specifies Ubuntu 20.04 as the base image.

\* `CMD \["echo", "Hello World"]`: Defines the default command executed when the container starts.



The CMD instruction uses exec form, which executes the specified process directly.



\## Build the Image



```cmd

docker build . -t cddo-hello-world

```



The image was successfully built and tagged as `cddo-hello-world:latest`.



\## Verify the Image



```cmd

docker images

```



The image appeared successfully in the local Docker image list.



\## Run the Container



```cmd

docker run cddo-hello-world

```



Expected output:



```text

Hello World

```



\## Verify the Container



```cmd

docker ps -a

```



The container appeared with the status:



```text

Exited (0)

```



Exit code 0 indicates successful execution.



\## Key Lessons Learned



1\. Dockerfiles define instructions for building images.

2\. Base images provide the foundation for custom images.

3\. Docker images are used to create containers.

4\. CMD defines the default command executed when a container starts.

5\. Containers stop when their main process terminates.

6\. `docker images` lists images, while `docker ps -a` lists containers.

7\. Exit code 0 indicates successful execution.



\## Completion Status



Exercise 01 successfully completed.



The image was built, the container executed successfully, and the expected output was verified.



