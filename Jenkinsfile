pipeline {
    // install golang 1.14 on Jenkins node
    agent any
    tools {
        go 'jenkins_golang'
    }
    // environment {
    //     GO114MODULE = 'on'
    //     CGO_ENABLED = 0 
    //     GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    // }
    stages {
        stage("Build") {
            steps {
                echo 'BUILD EXECUTION STARTED'
                sh 'docker build . -t katuang/golang-service'
            }
        }
        stage('Publish') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
                sh 'docker push katuang/golang-service'
                }
            }
        }
        stage('Deploy') {
           steps {
               withKubeConfig([credentialsId: 'kubeconfig']) {
                sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
                sh 'chmod u+x ./kubectl'
                sh './kubectl apply -f apps.yaml --insecure-skip-tls-verify'
               }
           }
        }
    }
}