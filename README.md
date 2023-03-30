Setting up a Node.js app on AWS EC2 using Jenkins and Docker


This tutorial will guide you through the process of setting up a Node.js app on an AWS EC2 virtual instance using Jenkins for continuous integration/continuous deployment (CI/CD) and Docker for containerization.

Prerequisites
Before you begin, make sure you have the following:

An AWS account with an EC2 instance running Ubuntu.
Jenkins installed on the EC2 instance.
Docker installed on the EC2 instance.
A Node.js app with a Dockerfile and a Jenkinsfile (for CI/CD).
Step 1: Create a Dockerfile
Create a Dockerfile in the root directory of your Node.js app. The Dockerfile should contain the following:

sql
Copy code
FROM node:latest
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
This Dockerfile will create a Docker image that runs your Node.js app on port 3000.

Step 2: Build the Docker image
To build the Docker image, run the following command in the root directory of your Node.js app:

arduino
Copy code
sudo docker build -t your-image-name .
Replace your-image-name with a name for your Docker image.

Step 3: Test the Docker image
To test the Docker image, run the following command:

arduino
Copy code
sudo docker run -p 3000:3000 your-image-name
This will start a container running your Node.js app on port 3000. You should be able to access the app by visiting http://your-ec2-instance-public-ip:3000 in a web browser.

Step 4: Create a Jenkins pipeline
Create a Jenkinsfile in the root directory of your Node.js app. The Jenkinsfile should contain the following:

typescript
Copy code
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'sudo docker build -t your-image-name .'
            }
        }
        stage('Test') {
            steps {
                sh 'sudo docker run your-image-name npm test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'sudo docker login -u AWS -p $(aws ecr get-login-password --region your-region) your-aws-account-id.dkr.ecr.your-region.amazonaws.com'
                sh 'sudo docker tag your-image-name your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repository-name:latest'
                sh 'sudo docker push your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repository-name:latest'
            }
        }
    }
}
This Jenkinsfile defines a Jenkins pipeline with three stages: Build, Test, and Deploy. In the Build stage, the Docker image is built. In the Test stage, the Docker image is run and the app is tested. In the Deploy stage, the Docker image is tagged and pushed to an AWS Elastic Container Registry (ECR) repository.

Step 5: Configure Jenkins
In Jenkins, create a new pipeline job and configure it to use your Git repository. In the pipeline configuration, specify the Jenkinsfile you created in Step 4.

Step 6: Run the pipeline
Run the pipeline in Jenkins. If the pipeline completes successfully, the Docker image should be pushed to your AWS ECR repository.

Conclusion
Congratulations, you have successfully set up a Node
