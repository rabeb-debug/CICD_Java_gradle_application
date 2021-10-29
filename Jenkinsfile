pipeline {

  
  agent any
  environment{
        VERSION = "${env.BUILD_ID}"
    }
  stages {
  
    stage('Sonar quality check') {
        agent {
            docker {
                image 'openjdk:11'
            }
        }
        steps {
            script {
                withSonarQubeEnv(credentialsId: 'sonar-token') {
                   sh 'chmod +x gradlew'
                   sh './gradlew '
                }


            }
        
      }
    }
          stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 34.125.214.226:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 192.168.205.10:8083 
                                docker push  192.168.205.10:8083/springapp:${VERSION}
                                docker rmi 192.168.205.10:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
  }}