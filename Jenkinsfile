pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo ' Build the source code'
                sh 'mvn install -DskipTests'
            }
        }
        stage('UnitTest') {
            steps {
                echo ' Running Unit Test'
                sh 'mvn clean test'
            }
        }
  stage('SonarQube analysis') {
    environment {
      SCANNER_HOME = tool 'sonar-scanner'
    }
    steps {
        withSonarQubeEnv('sonar-server') {
            sh '$SCANNER_HOME/bin/sonar-scanner'
       }
     }
}
   stage('SQuality Gate') {
     steps {
       timeout(time: 1, unit: 'MINUTES') {
       waitForQualityGate abortPipeline: true
       }
  }
}
    }
}
