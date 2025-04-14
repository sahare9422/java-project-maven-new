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
          stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
                    withMaven(maven: 'apache-maven-3.6.3'){  
                        sh 'mvn sonar:sonar -Dsonar.projectKey=java-project-maven-new -Dsonar.sources=./src/main/'
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarScanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
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
