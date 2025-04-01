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
                         sh "/usr/bin/mvn test"
                    }
               }
          }
     
    
stage("Package") {
     steps {
          withMaven(maven : 'apache-maven-3.6.3'){
               sh "/usr/bin/mvn package"
          }
     }
}
stage("Docker build") {
     steps {
           withMaven(maven : 'apache-maven-3.6.3'){
               sh "docker build -t deepak_tomcat ."
           }
     }
}

stage("Deploy to staging") {
     steps {
          
          sh "docker stop \$(docker ps -qa)"
          sh "docker rm \$(docker ps -qa)"
          sh "docker run -d -it -v /var/lib/jenkins/workspace/tomcat-cont/target/:/usr/local/tomcat/webapps/ -p 8091:8080 --name Testtomcat deepak_tomcat"
     }
}

     }
  post {
     always {
          sh "echo 'I did It'"
     }
}
}
