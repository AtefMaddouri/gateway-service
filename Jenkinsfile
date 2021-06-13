#!groovy

pipeline {
    environment {
        JAVA_TOOL_OPTIONS = "-Duser.home=/var/maven"
    }

    agent any

    tools {
        maven "maven:3.6.3" // You need to add a maven with name "3.6.0" in the Global Tools Configuration page
    }
    // agent {
    //     any {
    //         image "maven:3.6.3-jdk-8-alpine"
    //         label "docker"
    //         args "-v /tmp/maven:/var/maven/.m2 -e MAVEN_CONFIG=/var/maven/.m2"
    //     }
    // }

    stages {
        stage("Build") {
            steps {
                sh "mvn -version"
                sh "mvn clean install -DskipTests"
            }
        }
        stage("run image on server") {
            steps {
                sh "docker stop gateway-service || true"
                sh "docker rm image gateway-service || true"
                sh "docker run --name gateway-service --link config-service --link discovery-service -d -p 8761:8761 --restart always atef/gateway-service:1.0"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}