pipeline {
    // install golang 1.14 on Jenkins node
    agent any
    tools {
        'jenkins_golang'
    }
    // environment {
    //     GO114MODULE = 'on'
    //     CGO_ENABLED = 0 
    //     GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    // }
    stages {
        stage("build") {
            steps {
                echo 'BUILD EXECUTION STARTED'
                sh 'go mod download'
                sh 'go build -o docker-gs-ping'
                sh 'docker build . -t katuang/golang-service'
            }
        }
        stage('deliver') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
                sh 'docker push katuang/golang-service'
                }
            }
        }
    }
}