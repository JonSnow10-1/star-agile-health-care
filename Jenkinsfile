pipeline {
  agent any
  tools {
    maven 'M2_HOME'
  }
  environment {
    AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
  }
  stages {
    stage('Git Checkout') {
      steps {
        echo 'This stage is to clone the repository from GitHub'
        git branch: 'master', url: 'https://github.com/JonSnow10-1/star-agile-health-care.git'
      }
    }
    stage('Create Package') {
      steps {
        echo 'This stage will compile, test, package the Healthcare application'
        sh 'mvn package'
      }
    }
    stage('Generate Test Report') {
      steps {
        echo 'This stage generate Test report using TestNG'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/HealthCare-Project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
      }
    }
    stage('Create Docker Image') {
      steps {
        echo 'This stage will create the docker image of the Healthcare application'
        sh 'docker build -t ganesh36/healthcare:1.0 .'
      }
    }
    stage('DockerHub Login') {
      steps {
        echo 'This stage will log into DockerHub'
        withCredentials([usernamePassword(credentialsId: 'dockerlogin', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
          sh 'docker login -u ${dockeruser} -p ${dockerpass}'
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        echo 'This stage will push the Docker Image'
        sh 'docker push ganesh36/healthcare:1.0'
      }
    }
  }
}
