#!/usr/bin/env groovy

pipeline {
  agent any
  triggers {
    pollSCM('5,35 * * * *')
  }
  stages {
    stage ('Checkout') {
      steps {
        git url: 'https://github.com/LinuxLibrary/jgsu-spring-petclinic', branch: 'master'
      }
    }
    parallel {
      stage ('Build') {
        steps {
          sh './mvnw compile'
        }
      }
      stage ('Test') {
        steps {
          sh './mvnw test'
        }
      }
      stage ('Package') {
        steps {
          sh './mvnw package'
        }
      }
    }
  }
  post {
    success {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
      emailext subject: "Job: \'${JOB_NAME}\' (${BUILD_NUMBER}) ${currentBuild.result}",
        body: "\'${JOB_NAME}\' job has completed with status ${currentBuild.result} \
        \nPlease refer to the build url: ${BUILD_URL}",
        attachLog: true,
        compressLog: true,
        from: 'admin@test.com',
        to: 'arjun@test.com'
    }
  }
}