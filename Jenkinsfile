pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'patch-1', url:'https://github.com/cameronmcnz/rock-paper-scissors.git'
 
                // Run Maven on a Unix agent.
              sh "mvn clean package"

                // To run Maven on a Windows agent, use
               // bat "mvn -Dmaven.test.failure.ignore=true clean install"
            }
        }

             stage("deploy-dev"){
                 steps{
                    // If Maven was able to run the tests, even if some of the test
                    // failed, record the test results and archive the jar file.
                    success {
                           sshagent(['tomcat-dev']){
                               sh 'scp -o StrictHostKeyChecking=no **/*.war ec2-user@172.31.41.47:/usr/share/tomcat/webapps/'
                           }
                    }
                }
            }
        }
    }
}
