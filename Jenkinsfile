pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
        registry = "rabebnefzi/app"
        registryCredential = 'dockerhub'
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
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

                    sh'dockerImage = docker build -t registry:${VERSION} . '
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                                }
                    sh'docker rmi dockerImage'            
                    
                    }
                }
            }
        }
    }
