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
       stage('SonarScan') {
            steps {
                echo 'Running Sonar Scan'
            }
        }
    }
      post{
        always {
            echo 'always block'
        }
        success {
            echo 'build succeeded'
        }
        failure {
            echo 'build failed'
        }
    }
}
