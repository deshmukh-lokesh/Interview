Architecture:

<img width="1000" height="382" alt="image" src="https://github.com/user-attachments/assets/18dfc758-f924-4a44-bdcf-f58ec518c8ea" />



Dockerfile workflow: 

Dockerfile->docker build-> docker image->docker run-> container

Common Dockerfile Instructions
1.FROM  == Defines the base image.
2.RUN == Executes commands during image build.
3.COPY == Copies files from host machine into image.
4.ADD == Automatically extracts archive.Can handle URLs
5.WORDIR == Sets working directory.
6.CMD == Defines default command when container starts.
                   executed automatically. Only one CMD should exist,
7.ENTRYPOINT == Forces container to run a particular command. Difficult to override.
8.ENV == Sets environment variables.
9.EXPOSE == Documents container port.
10.USER == Defines which user runs the container.
11.VOLUME == Creates persistent storage.


Multistage docker file
Multi-stage Docker build uses multiple FROM statements. The first stage builds the application, and the final stage copies only the required artifacts, resulting in smaller, faster, and more secure Docker images.

Why Use Multi-Stage Builds?
	• Smaller images
	• Better security
	• Faster deployment
	• Cleaner Dockerfiles

Distroless images are minimal Docker images that contain only the application and required runtime dependencies, without operating system utilities like bash, apt, yum, curl, or wget. They provide smaller image sizes, improved security, and are commonly used in production Kubernetes environments along with multi-stage Docker builds.

Alpine vs Distroless
Feature	Alpine	Distroless
Shell Available	✅ Yes	❌ No
Package Manager	✅ apk	❌ No
Debugging Easy	✅ Yes	❌ Hard
Image Size	Small	Smaller
Security	Good	Better

A Distroless Image is a Docker image that contains:
✅ Your application
✅ Required runtime (Java, Python, Node.js, etc.)
❌ No Linux shell (bash, sh)
❌ No package manager (yum, apt, apk)
❌ No debugging tools (curl, wget, vim)
The goal is to make images smaller, faster, and more secure.

Docker Bind Mount
A Bind Mount is a way to connect a directory or file from the host machine directly into a Docker container.
A bind mount maps a file or directory from the host machine directly into a Docker container, allowing both host and container to access the same data.


A Docker Volume is a persistent storage mechanism that stores data outside the container, allowing data to survive container deletion.

What are the Types of Docker Volumes?
	1. Named Volume
	2. Bind Mount
	3. Anonymous Volume

Quick Comparison
Type	Managed By	Example
Named Volume	Docker	myvolume:/data
Bind Mount	User	/home/rke/data:/data
Anonymous Volume	Docker (random name)	/data

Volume Commands

	1. docker volume create myvolume-> will create a volume
	2. docker volume ls -> list of the volume
	3. docker volume inspect myvolume -> Shows volume details (metadata, mount path, driver)
	4. docker volume rm myvolume -> Removes a specific volume
	5. docker volume prune -> Force Remove Unused Volumes
	
Backup of volume
	1. Make a tar



Types of Docker Networks

1. Bridge Network ->  default network
2. Host Network
3. None Network
4. Overlay Network
5. Macvlan Network

	

