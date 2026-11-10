\# Exercise 02: Customised Docker Base Image



\## 1. Objective



The objective of this exercise was to understand how to create a customised Docker base image using Ubuntu 22.04 and install additional packages required for a development environment.



The exercise demonstrated how Dockerfile instructions such as `FROM`, `RUN`, `ENV` and `CMD` work.



\## 2. Original Base Image Investigation



The original image was Ubuntu 22.04.



Command:



```cmd

docker run --rm ubuntu:22.04 python3 -V

```



Result:



```text

exec: "python3": executable file not found in $PATH

```



This established that Python 3 was not installed in the original Ubuntu image.



An interactive container was also started:



```cmd

docker run --rm -it ubuntu:22.04 /bin/bash

```



The following commands were used to investigate the available packages:



```bash

whoami

python3 -V

python -V

curl --version

wget --version

```



Findings:



\* The container ran as `root`.

\* Python 3 was not installed.

\* Python was not installed.

\* curl was not installed.

\* wget was not installed.



This demonstrated that a base image does not necessarily contain every package required by an application.



\## 3. Dockerfile Configuration



The Dockerfile uses Ubuntu 22.04 as its foundation.



```dockerfile

FROM ubuntu:22.04



RUN apt-get update -y \&\& \\

&#x20;   DEBIAN\_FRONTEND=noninteractive apt-get install -y --no-install-recommends nano vim emacs \\

&#x20;   curl wget iproute2



RUN DEBIAN\_FRONTEND=noninteractive apt-get install -y --no-install-recommends python3 python3-pip



ENV TZ=Europe/London

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \&\& echo $TZ > /etc/timezone



CMD \["echo", "This is just a base image"]

```



\### Explanation of Dockerfile instructions



\*\*FROM\*\*



Specifies the base image used to build the customised image.



\*\*RUN\*\*



Executes commands during the image build process. It was used to update Ubuntu's package lists and install additional software.



\*\*ENV\*\*



Defines an environment variable. In this exercise, it sets the timezone to Europe/London.



\*\*CMD\*\*



Defines the default command executed when a container starts without another command being specified.



\### Packages installed



| Package     | Purpose                         |

| ----------- | ------------------------------- |

| nano        | Text editor                     |

| vim         | Text editor                     |

| emacs       | Text editor                     |

| curl        | HTTP requests and data transfer |

| wget        | Downloading files               |

| iproute2    | Linux networking utilities      |

| python3     | Python interpreter              |

| python3-pip | Python package manager          |



\## 4. Building the Customised Image



Command:



```cmd

docker build . -t cddo-base-image

```



Result:



Build completed successfully.



Image name:



```text

cddo-base-image:latest

```



Image ID:



```text

50dc2836e949

```



Docker Desktop reported:



\* Disk usage: 754 MB

\* Content size: 197 MB



These are different storage measurements reported by Docker Desktop.



\## 5. Image Verification



The image was verified using:



```cmd

docker images cddo-base-image

```



The image appeared successfully in the local Docker image listing.



\## 6. Package Verification



The following commands were executed:



```cmd

docker run --rm cddo-base-image python3 -V

docker run --rm cddo-base-image pip3 --version

docker run --rm cddo-base-image curl --version

docker run --rm cddo-base-image wget --version

```



Results:



| Component | Verified Version | Status |

| --------- | ---------------- | ------ |

| Python 3  | 3.10.12          | Passed |

| pip       | 22.0.2           | Passed |

| curl      | 7.81.0           | Passed |

| wget      | 1.21.2           | Passed |



\## 7. Default Command Verification



Command:



```cmd

docker run cddo-base-image

```



Output:



```text

This is just a base image

```



The container's main process completed successfully.



Unlike containers started with `--rm`, this container remains available in the stopped-container listing.



\## 8. Interactive Container Verification



Command:



```cmd

docker run --rm -it cddo-base-image /bin/bash

```



Additional verification:



```bash

whoami

python3 -V

curl --version

date

```



Results:



\* Current user: root

\* Python: 3.10.12

\* curl: 7.81.0

\* Timezone: Europe/London



This confirmed that the installed packages and timezone configuration were available inside containers created from the customised image.



\## 9. Important Concepts Learned



1\. A base image provides the foundation for a customised image.

2\. A minimal operating-system image may not include application dependencies.

3\. `apt-get update` refreshes package information.

4\. `apt-get install` installs packages and their dependencies.

5\. `RUN` executes during image building.

6\. `CMD` defines the default container startup command.

7\. `ENV` defines environment variables.

8\. Containers commonly run as root by default unless another user is configured.

9\. `docker run --rm` automatically removes a container after its main process exits.

10\. Customised base images improve consistency, repeatability and automation.



\## 10. DevOps Relevance



Customised base images allow DevOps engineers to standardise application environments.



Instead of repeatedly installing dependencies whenever a container is created, engineers can prepare an image containing the required software and configurations.



Benefits include:



\* Consistent environments.

\* Faster container startup.

\* Repeatable builds.

\* Easier automation through CI/CD pipelines.

\* Centralised dependency management.



Production images should also be kept lean, regularly updated and scanned for vulnerabilities.



\## 11. Completion Status



\*\*Exercise 02: Completed\*\*



The customised Ubuntu 22.04 image was successfully built, and all required package and configuration checks passed.



