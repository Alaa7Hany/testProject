# EC2 Jenkins Docker Pipeline Project

## Project Overview

This practice project demonstrates how to configure an AWS EC2 instance using Ansible to run Jenkins as a Docker container. The Jenkins pipeline is linked to a GitHub repository and is configured to automate the build, push, and deployment of a simple HTML application using Docker.

## Project Workflow

1. **Provisioning EC2 Instance**:
   - Ansible is used to configure the EC2 instance with necessary dependencies, including Docker.
   - Jenkins is set up inside a Docker container on the EC2 instance.

2. **Jenkins Pipeline**:
   The Jenkins pipeline is linked to the GitHub repository and automates the following stages:
   
   - **Building the Docker Image**: Builds a Docker image from the HTML and Dockerfile in the repository.
   - **Pushing the Image to Docker Hub**: Pushes the built Docker image to Docker Hub.
   - **Deploying the Application**: Deploys the application by running a Docker container from the pushed image.


