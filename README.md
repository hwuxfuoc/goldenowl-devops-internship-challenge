# Golden Owl DevOps Internship - Technical Test
At Golden Owl, we believe in treating infrastructure as code and automating resource provisioning to the fullest extent possible. 

In this technical test, we challenge you to create a robust CI build pipeline using GitHub Actions. You have the freedom to complete this test in your local environment.

## Your Mission 🌟
Your mission, should you choose to accept it, is to craft a CI job that:
1. Forks this repository to your personal GitHub account.
2. Dockerizes a Node.js application.
3. Establishes an automated CI/CD build process using GitHub Actions workflow and a container registry service such as DockerHub or Amazon Elastic Container Registry (ECR) or similar services.
4. Initiates CI tests automatically when changes are pushed to the feature branch on GitHub.
5. Utilizes GitHub Actions for Continuous Deployment (CD) to deploy the application to major cloud providers like AWS EC2, AWS ECS or Google Cloud (please submit the deployment link).
## Nice to have 🎨
We would be genuinely delighted if you could complement your submission with a `visual flow diagram`, illustrating the sequence of tasks you performed, including the implementation of a `load balancer` and `auto scaling` for the deployed application. This additional touch would greatly enhance our understanding and appreciation of your work.

Reference tools for creating visual flow diagrams:
- https://www.drawio.com/
- https://excalidraw.com/
- https://www.eraser.io/
  
Including a visual representation of your workflow will provide valuable insights into your approach and make your submission stand out. Thank you for considering this enhancement! 
## The Bigger Picture 🌏
This test is designed to evaluate your ability to implement modern automated infrastructure practices while demonstrating a basic understanding of Docker containers. In your solution, we encourage you to prioritize readability, maintainability, and the principles of DevOps.

 ## Submission Guidelines 📬
Your solution should be showcased in a public GitHub repository. We encourage you to commit early and often. We prefer to see a history of iterative progress rather than a single massive push. When you've completed the assignment, kindly share the URL of your repository with us.

 ## Running the Node.js Application Locally  🏃‍♂️
 This is a Node.js application, and running it locally is straightforward:
- Navigate to the `src` directory by executing `cd src`.
- Install the project's dependencies listed in the package.json file by running `npm i`.
- Execute `npm test` to run the application's tests.
- Start the HTTP server with `npm start`.

You can test it using the following command:
  
```shell
curl localhost:3000
```
You should receive the following response:
```json
{"message":"Welcome warriors to Golden Owl!"}
```

Are you ready to embark on this DevOps journey with us? 🚀 Best of luck with your assignment! 🌟

# My Deployment 🚀

## Live URL
- **EC2**: [`goldenowl-server`](http://54.251.64.50:3000)
- **ALB**: [`goldenowl-alb`](http://goldenowl-alb-1521996269.ap-southeast-1.elb.amazonaws.com)
- **DockerHub**: [`hwuxfuoc/goldenowl-app`](https://hub.docker.com/r/hwuxfuoc/goldenowl-app)

![EC2 Response](./diagrams/ec2.png)
![ALB Response](./diagrams/alb.png)

## What I Built

### 1. Dockerized the Node.js Application
Multi-stage Docker build using `node:18-alpine`. Separates build from runtime to minimize image size.

### 2. CI Pipeline (GitHub Actions)
Triggers on push to `feature/**` and PRs to `master`. Three sequential jobs:

- **Lint**: ESLint
- **Test**: Jest
- **Build & Push**: Docker image pushed to DockerHub with `latest` and commit SHA tags

### 3. CD Pipeline (GitHub Actions)
Triggers on merge to `master`:

1. SSH into EC2
2. Pull latest image from DockerHub
3. Replace the running container (`--restart unless-stopped`)

### 4. Infrastructure
- **AWS EC2**: Hosts the containerized Node.js application on port 3000
- **AWS ALB**: Accepts public HTTP traffic on port 80 and forwards it to the EC2 target group
- **AWS Auto Scaling Group**: Maintains a desired capacity of 2 instances, scales up to 4 when CPU exceeds 70%, and scales back down when load drops

## Architecture Diagram

End-to-end overview of the CI/CD pipeline and AWS infrastructure.


![CI/CD Flow](./diagrams/ci-cd-infrastructure.png)

## CI/CD in Action

GitHub Actions workflows running automatically on push and merge events.


![CI Pipeline](./diagrams/ci-pipeline.png)
![CD Pipeline](./diagrams/cd-pipeline.png)

## Docker Registry

Image is automatically built and pushed to DockerHub on every CI run.


![DockerHub](./diagrams/dockerhub.png)

## AWS Infrastructure
### EC2 Instance
Ubuntu 24.04 on `t3.micro`, running the app inside a Docker container on port 3000.

![EC2 Instance](./diagrams/aws-ec2.png)

### Application Load Balancer
Internet-facing ALB listening on port 80, forwarding traffic to EC2 instances on port 3000.

![ALB](./diagrams/aws-alb.png)

### Auto Scaling Group
Scales EC2 instances between 1–4 based on CPU usage. Desired capacity: 2.

![Auto Scaling Group](./diagrams/aws-asg.png)

### Target Group (Healthy Instances)
Health check on `GET /` and instances must return 200 to receive traffic.

![Target Group](./diagrams/aws-target-group.png)
