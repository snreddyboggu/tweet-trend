def registry = 'https://qtkaja.jfrog.io/'
def imageName = 'valaxy01.jfrog.io/valaxy-docker-local/ttrend'
def version   = '2.1.2'
pipeline{
    agent   { label 'mavenagent' }
    environment{
        PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
    }
    stages{

        stage('build'){
            
            steps{
                echo 'build started'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo 'build finsihed'
            }

        }
        stage('Unit Test'){
            steps{
                echo 'unit test started '
                sh 'mvn surefire-report:report'
                echo 'unit test completed'
            }

        }

         stage('SonarQube analysis') {
            environment{
             scannerHome = tool 'SonarScanner 4.0';

            }
            steps{
                echo 'sonar started'
                withSonarQubeEnv('sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner"
            echo 'sonar completed'
    }
            }

            
  }
 
        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artifactory"
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

       
    }

}
