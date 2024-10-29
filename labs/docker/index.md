---
title: "Docker Fundamentals"
---
---

# **Docker Fundamentals Lab**

### **Introduction**
This lab guides you through deploying an AWS EC2 instance, installing and configuring Docker, and containerizing a Python application. You will learn essential skills for setting up infrastructure, handling containerized applications, and testing access within a secure network configuration, focusing on local and external access to deployed services.

---

## **Lab Objectives**
By the end of this lab, you will:
1. Create a Linux EC2 instance.
2. Install Docker.
3. Build a Docker image.
4. Start a container
5. Enable external access

---

## **Log in to the AWS Management Console**
1. Log in to the AWS Management Console using your credentials.
2. In the top-right corner of the page, ensure that you are in the **`us-west-1` (N. California)** region.

---

## Create an EC2 Instance:

1. In the AWS Management Console, type **EC2** in the search bar and select **EC2** from the results.
2. Click **Launch Instance**.
3. Provide a name for your instance, such as `docker`.
4. Under **Amazon Machine Image (AMI)**, select **Ubuntu**.
5. Under **Instance type**, choose **t2.small**.
6. Scroll down to **Key pair (login)**, and select an existing key pair to allow SSH access to your instance.
7. Scroll down to **Network settings**, confirm the VM is being deployed to the default VPC, and configure a security group that allows **SSH** and. **HTTP** access.
8. Leave the **Storage** settings as default and click **Launch instance**.

---

## **Install Docker**

### Log into the new instance:
1. Log into the new instance as the `ubuntu` user.
2. Update the `apt` cache and Install Docker

   ```bash
   sudo apt update && sudo apt install docker.io docker-buildx -y
   ```
3. Add your user to the `docker` group

   ```bash
   sudo usermod -aG docker $USER
   ```
4. Log out and back into the instance to apply the new user group permissions.
5. Confirm you can interact with `docker` as the `ubuntu` user. 

   ```bash
   docker ps
   ```

   This command should show there are no running containers.

## Containerize Python application:
1. Clone the python application from GitHub.

   ```bash
   git clone https://github.com/estebanx64/python-docker-example
   ```

2. Enter the directory and create a `Dockerfile` with the following:

   ```dockerfile
   # syntax=docker/dockerfile:1
   
   # Comments are provided throughout this file to help you get started.
   # If you need more help, visit the Dockerfile reference guide at
   # https://docs.docker.com/go/dockerfile-reference/
   
   # Want to help us make this template better? Share your feedback here: https://forms.gle/ybq9Krt8jtBL3iCk7
   
   ARG PYTHON_VERSION=3.11.4
   FROM python:${PYTHON_VERSION}-slim AS base
   
   # Prevents Python from writing pyc files.
   ENV PYTHONDONTWRITEBYTECODE=1
   
   # Keeps Python from buffering stdout and stderr to avoid situations where
   # the application crashes without emitting any logs due to buffering.
   ENV PYTHONUNBUFFERED=1
   
   WORKDIR /app
   
   # Create a non-privileged user that the app will run under.
   # See https://docs.docker.com/go/dockerfile-user-best-practices/
   ARG UID=10001
   RUN adduser \
       --disabled-password \
       --gecos "" \
       --home "/nonexistent" \
       --shell "/sbin/nologin" \
       --no-create-home \
       --uid "${UID}" \
       appuser
   
   # Download dependencies as a separate step to take advantage of Docker's caching.
   # Leverage a cache mount to /root/.cache/pip to speed up subsequent builds.
   # Leverage a bind mount to requirements.txt to avoid having to copy them into
   # into this layer.
   RUN --mount=type=cache,target=/root/.cache/pip \
       --mount=type=bind,source=requirements.txt,target=requirements.txt \
       python -m pip install -r requirements.txt
   
   # Switch to the non-privileged user to run the application.
   USER appuser
   
   # Copy the source code into the container.
   COPY . .
   
   # Expose the port that the application listens on.
   EXPOSE 8000
   
   # Run the application.
   CMD python3 -m uvicorn app:app --host=0.0.0.0 --port=8000
   ```

3. Build the container image (**NOTE: The `.` is required at the end**)

   ```bash
   DOCKER_BUILDKIT=1 docker build -t python-demo .
   ```

4. Confirm the image was built

   ```bash
   docker images
   ```

   You will see output similar to this: 

   ```
   REPOSITORY           TAG           IMAGE ID       CREATED          SIZE
   python-demo          latest        3a17208e32e7   7 minutes ago    203MB
   <none>               <none>        1502d7ec819f   10 minutes ago   149MB
   python               3.11.4-slim   596e0d6b34df   15 months ago    149MB
   ```

5. Create a container from the image

   ```bash
   docker run -d -p 8000:8000 python-demo
   ```

6. Confirm the container is running 

   ```bash
   docker ps 
   ```

   You should see something like this: 

   ```
   CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS         PORTS                                       NAMES
   bf32d5d953d2   aslaen/python-demo   "/bin/sh -c 'python3…"   3 seconds ago   Up 2 seconds   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   focused_wu
   ```

## Test the application:

1. Run the following command in the terminal to test the application

   ```bash
   curl localhost:8000
   ```

   You should see the message "Hello World"

1. Try to access the application externally by going to the instance's public IP address in a browser on port 8000. It will fail because we did not allow port 8000 in the security group. 

---

## **Challenge lab**

There are multiple options for accessing the application externally. Each student should solve this issue and then present their solution to the class. 

## Conclusion
In this lab, you successfully:
- Provisioned an EC2 instance with Ubuntu, configured for SSH and HTTP access.
- Installed Docker, containerized a Python application, and successfully ran it on port 8000.
- Tested local application access.
- Solved a challenge to enable external access to the application.
