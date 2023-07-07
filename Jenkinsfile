def registry = 'https://yogitha143.jfrog.io'

def version   = '2.1.2'
pipeline{
    agent   any
    environment{
        PATH = "/opt/apache-maven-3.9.3/bin:$PATH"
    }
    stages{

        stage('build'){
            
            steps{
                echo 'build started'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo 'build finsihed'
            }

        }
       
        }   
    }

        
 
        
                    

       
    }

}
