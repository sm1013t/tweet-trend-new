pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo ' Build the source code'
                // sh 'mvn install -DskipTests'
                sh 'mvn clean deploy -DskipTests'
            }
        }
        stage('UnitTest') {
            steps {
                echo ' Running Unit Test'
                sh 'mvn surefile-report:report'
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
    }
}
