# Golang service

This service use jenkins pipeline for build and deployment. There are three steps for the pipeline. The process is defined in Jenkinsfile

###### Build 
Dockerfile is used to build the service that generate container image.

###### Publish
Publish the container image to dockerhub

###### Deploy
apps.yaml is kubernetes manifest file for deployment.
