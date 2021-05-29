#!/usr/bin/env groovy

pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ('Checkout') {
            steps {
                git url: 'https://github.com/LinuxLibrary/jgsu-spring-petclinic', branch: 'master'
            }
        }
        stage ('Build') {
            steps {
                sh './mvnw compile'
            }
        }
    }
}