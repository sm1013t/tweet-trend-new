def registry = 'https://attorg.jfrog.io/'
def imageName = 'attorg.jfrog.io/my-docker-repo-docker-local/ttrend'
def version   = '2.1.2'
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo ' Build the source code'
                // sh 'mvn install -DskipTests'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
            }
        }
        /*
        stage('UnitTest') {
            steps {
                echo ' Running Unit Test'
                sh 'mvn surefire-report:report'
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
        */
        
        stage('Jar Publish') {
             steps {
                    script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artifactory_cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
               }
          }
        stage(" Docker Build ") {
              steps {
                script {
                       echo '<--------------- Docker Build Started --------------->'
                       app = docker.build(imageName+":"+version)
                       echo '<--------------- Docker Build Ends --------------->'
                }
              }
        }
        stage (" Docker Publish "){
            steps {
                script {
                   echo '<--------------- Docker Publish Started --------------->'  
                    docker.withRegistry(registry, 'artifactory_cred'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
            }
        }
}
}
