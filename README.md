# Docker Java Sample Web Application (Kubernetes Deployment)

## Project Overview

This project demonstrates how to build, containerize, and deploy a Java web application using Docker and Kubernetes.

The application is a simple Java Servlet-based web service that runs on Apache Tomcat. It accepts a name parameter via HTTP request and returns a greeting message.

Example:
http://<host>:<port>/?name=Abhishek

Response:
Hello Abhishek

If no name is provided, the application returns:
Hello Guest

---

## Project Architecture

Java Servlet Application
        ↓
Maven Build (WAR)
        ↓
Docker Image
        ↓
Tomcat Container
        ↓
Kubernetes Deployment
        ↓
Kubernetes Service / Ingress
        ↓
Browser / API Request

---

## Prerequisites

Make sure the following tools are installed:

- Java 8+
- Maven
- Docker
- Kubernetes CLI (kubectl)
- Minikube

Install kubectl:
sudo snap install kubectl --classic

Install Minikube (Linux):
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

Verify installation:
kubectl version --client
minikube version
docker --version

---

## Step 1: Build the Application

Build the WAR package using Maven.
mvn clean package

Output file:
target/docker-java-sample-webapp-1.0-SNAPSHOT.war

---

## Step 2: Build the Docker Image

The Dockerfile uses a Tomcat base image and deploys the application as ROOT.war.

Build the Docker image:
docker build -t docker-java-sample:latest .

Verify:
docker images

---

## Step 3: Start Minikube Cluster

Start a local Kubernetes cluster using Minikube.
minikube start

Configure Docker to use Minikube's internal Docker daemon:
eval $(minikube docker-env)

Build the image inside Minikube:
docker build -t docker-java-sample:latest .

---

## Step 4: Deploy to Kubernetes

Apply the deployment configuration.
kubectl apply -f k8s/deployment.yaml

(Optional) Apply Ingress configuration:
kubectl apply -f k8s/ingress.yaml

Verify deployment:
kubectl get pods
kubectl get services

---

## Step 5: Access the Application

Retrieve the service URL:
minikube service docker-java-svc --url

Example output:
http://192.168.49.2:30080

Test using browser or curl.

With name parameter:
curl "http://192.168.49.2:30080/?name=YourName"

Response:
Hello YourName

Without parameter:
http://192.168.49.2:30080/

Response:
Hello Guest

---

## Kubernetes Configuration Details

### Deployment

- Replicas: 2
- Container Image: docker-java-sample:latest
- Runtime: Apache Tomcat (tomcat:9-jre8)

Health Checks:
- Liveness Probe: /?name=health
- Readiness Probe: /?name=health

### Service

Service Type:
NodePort

Exposed Port:
30080

This port can be modified inside:
k8s/deployment.yaml

---

## Technologies Used

- Java Servlet
- Apache Tomcat
- Maven
- Docker
- Kubernetes
- Minikube

---

## Learning Objectives

This project demonstrates:
- Building Java web applications
- Creating Docker containers
- Running applications inside Tomcat
- Deploying containers to Kubernetes
- Exposing services using NodePort and Ingress
- Running applications locally using Minikube

---