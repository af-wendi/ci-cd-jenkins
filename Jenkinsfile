
Search
Write
Sign up

Sign in



Setting up a CI/CD Pipeline Process with Jenkins and Docker in AWS
Ulises Magana
Cloud Native Daily
Ulises Magana

·
Follow

Published in
Cloud Native Daily

·
12 min read
·
May 10
49


1




Part 2: Monitoring Made Easy: Enhancing CI/CD with Splunk and Jenkins Integration


In today’s fast-paced development environment, it is crucial to have a robust Continuous Integration and Continuous Delivery (CI/CD) pipeline to quickly and efficiently deploy code changes to production. In this article, we will explore how to set up a CI/CD pipeline using Jenkins and Docker by building a simple Flask application, testing it, and deploying it to Docker Hub.

Objectives
Containerize a simple Flask application using Docker to make it more portable and scalable.
Use Git to manage the source code of the application to make it easier to collaborate and version-control changes.
Implement Infrastructure as Code for automated build, test, and deployment process using Jenkins pipeline script.
Ensure Continuous Integration of the application by configuring Jenkins to automatically build and test every time code changes are made.
Implement Continuous Delivery/Deployment by configuring Jenkins to automatically deploy the application to a Docker registry when the build and test phases are successful.
Key concepts and tools to be applied
Infrastructure as Code: Firstly, we will leverage Jenkins pipeline scripting to automate the build, test, and deployment process of our application. This pipeline script can be version-controlled and treated as code, making it easier to manage and reproduce the pipeline.

Continuous Integration: It is implemented using Jenkins, which will automatically build and test our application every time code changes are made. This will help identify issues early in the development process, allowing for faster feedback and remediation.

Continuous Delivery/Deployment: We will also use Jenkins to automatically deploy our application to a server or Docker registry when the build and test phases are successful, ensuring that the latest version of our application is always available to end-users. This ensures Continuous Delivery/Deployment and reduces the time-to-market for our application.

Continuous Testing: The Flask application will be continuously tested while the Docker image is run as part of the pipeline process, helping to identify any issues early in the development cycle.

Webhook Triggers: We will set up webhook triggers to automatically trigger pipeline builds whenever changes are pushed to the Git repository. This ensures that our pipeline is always up-to-date with the latest changes, and our application is continuously built and tested.

Docker: To make our application more portable and scalable, we will containerize it using Docker. Docker allows us to package our application and its dependencies into a single, portable unit that can be run consistently on any platform.

Git: We will use Git to manage the source code of our application, making it easier to collaborate and version-control changes.

Pre-requisites
AWS account and familiarity with AWS EC2.
Docker Hub account.
Familiarity with Git version control and experience with command line interface.
Github account.
Understanding of the Flask application framework and Docker containerization technology.
Project Structure
app/__init__.py — This file is used to mark the directory as a Python package and to define the Flask application object. It is in the same directory as the app.py file since when Flask looks for an application, it searches for the __init__.py by default. In this project, it is not necessary to include any content in this file.

app/app.py

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
This code defines a simple Flask application that has one route, the root route ‘/’. When a user navigates to the root route, the hello_world() function is executed, which returns a string "Hello, World!".

The first two lines of the code import the Flask class and create a new Flask application instance.
The @app.route('/') decorator defines the URL path to match for the following function.
The hello_world() function is decorated with @app.route('/'), which means that it will be called when the user navigates to the root URL path.
The final if __name__ == '__main__': block ensures that the app.run() function only runs if the script is executed directly, rather than being imported by another script.
The app.run() function starts the Flask application and makes it available on the network on the IP address 0.0.0.0 and the default port 5000. The debug=True argument enables debug mode, which allows the server to reload itself on code changes and provides helpful debugging messages.
Dockerfile

FROM python:3.9-alpine

WORKDIR /flask_app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

RUN pip install pytest

COPY app/ .

COPY tests/ app/tests/

CMD [ "python", "app.py" ]
It is used to create a Docker image for a Flask application. It uses the Python 3.9-alpine image as a base and sets the working directory to /flask_app. The requirements.txt file is copied into the image and the necessary dependencies are installed. Please note that the — no-cache-dir parameter in line 4 is used to tell pip not to use cache during the installation process, which can help reduce the size of the Docker image.

Then, pytest is installed for testing purposes. The application code is copied into the image along with the test directory. Finally, the command is set to run the Flask application by running app.py using the python command.

Jenkinsfile

pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t my-flask-app .'
        sh 'docker tag my-flask-app $DOCKER_BFLASK_IMAGE'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run my-flask-app python -m pytest app/tests/'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'docker push $DOCKER_BFLASK_IMAGE'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}