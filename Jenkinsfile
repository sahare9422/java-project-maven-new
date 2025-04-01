pipeline {
     agent any
     stages {
          stage("Compile") {
               steps {
                    withMaven(maven : 'apache-maven-3.6.3'){
                         sh "mvn clean compile"
                    }
               }
          }
          stage("Unit test") {
               steps {
                    withMaven(maven : 'apache-maven-3.6.3'){
                         sh "mvn test"
                    }
               }
          }
     
    
stage("Package") {
     steps {
          withMaven(maven : 'apache-maven-3.6.3'){
               sh "mvn package"
          }
     }
}
stage("Docker build") {
     steps {
           withMaven(maven : 'apache-maven-3.6.3'){
               sh "docker build -t hotstar:v1 ."
           }
     }
}

stage("Deploy to staging") {
     steps {
          
          sh "docker stop \$(docker ps -qa)"
          sh "docker rm \$(docker ps -qa)"
          sh "docker run -d -it -v /var/lib/jenkins/workspace/hotstar-cont/target/:/usr/local/tomcat/webapps/ -p 8081:8080 --name Hotstartomcat hotstar:v1"
     }
}

     }
  post {
     always {
          sh "echo 'I did It'"
     }
}
}
