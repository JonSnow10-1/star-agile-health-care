pipeline {
  agent any
  tools {
    maven 'M2_HOME'
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
  }
}
