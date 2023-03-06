pipeline{
    agent any

     tools{
        maven 'MAVEN_3.9.0'
     }
      stages{
        stage("build"){
            steps{
                script{
                    sh 'mvn install -Dv=${BUILD_NUMBER}'
                }

            }
        }
        stage("unit testing"){
            steps{
                
                sh 'mvn test'
            }
            post{
                success{
                     echo "junit testing is success,publishing report"
                     junit 'target/surefire-reports/*.xml'
                
                }
                failure{
                    echo "junit testing is failed"

                }
            }
        }
        stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'mysonar') {
                        sh "${tool("mysonar")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://18.180.157.138:9000\
                        -Dsonar.login=sqp_376ebffc91921f7fbeb0ee9a725814e14b1a4ebf"
    
                    }
                }
            }

        }
        stage("upload artifact"){
            steps{
               sh 'mvn -s settings.xml deploy -Dv=${BUILD_NUMBER}'
            }
        }
        stage("deployment"){
            agent{
                label 'any'
            }
              steps{
                script{
                   sh 'ansible-playbook -i inventory.yaml deployment_playbook.yaml -e "build_number=${BUILD_NUMBER}\"'
                }
                  
              }
        }
        
    }

}