pipeline {
    agent any

    environment {
       // unique_Id = UUID.randomUUID().toString()
          UUID uuid = UUID.randomUUID()
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'patch-1', url:'https://github.com/cameronmcnz/rock-paper-scissors.git'
                
                 sh 'rm -rf main'
                 sh 'mkdir main'
                 sh 'touch main/vars.html'
              // def  unique_Id = UUID.randomUUID().toString()
                // sh 'curl $unique_Id'
             
println "Random UUID: "+uuid
sh 'echo $uuid> main/vars.html'
                
               //  sh 'echo "Test Version: 1.0"> main/vars.html'

                // Run Maven on a Unix agent.
              sh "mvn clean package"

                // To run Maven on a Windows agent, use
               // bat "mvn -Dmaven.test.failure.ignore=true clean install"
            }
//  deploy adapters: [tomcat9(credentialsId: 'c4046d3f-d8d7-4771-9691-9d929fa08e9d', path: '', url: 'http://3.134.92.78:8080/')], contextPath: 'rpps', war: '.war'
    
        }
         stage('Deploy') {
             steps {
             deploy adapters: [tomcat9(credentialsId: 'c4046d3f-d8d7-4771-9691-9d929fa08e9d', path: '', url: 'http://localhost:8080/')], contextPath: 'rpps', war: '**/*.war'
             }
         }
         
          stage('Monitor') {
             steps {
                 
                  sh 'test -f main/vars.html'
                  
                 sh 'grep $uuid main/vars.html'
                 
                    sh '''url=\'http://localhost:8080/rpps\'
code=`curl -sL --connect-timeout 20 --max-time 30 -w "%{http_code}\\\\n" "$url" -o /dev/null`'''
                 
               
             }
         } 
         
    }
}
