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
        /*stage("Unit Testing") {
            steps{
                sh 'mvn test'
            }
            post {
                success{
                    echo "======Unit testing completed successfull, publishing report========="
                    junit 'target/surefire-reports/*.xml'
                }
                failure{
                    echo "==========unit test cases failed, report not published===="

                }
            }
        }*/

        stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'SONARQUBE') {
                        sh "${tool("SONARQUBE")}/bin/sonar-scanner \
                        -Dsonar.projectKey=java-maven-war-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://18.183.205.42:9000\
                        -Dsonar.login=sqp_1ccfcbc3a541695cdf212d9638fc67cd96ffe9a6"
    
                    }
                }
            }

        }
        stage("upload artifact"){
            steps{
               sh 'mvn -s settings.xml deploy -Dv=${BUILD_NUMBER}'
            }
        }
        /*stage("deployment"){
            agent{
                label 'any'
            }
              steps{
                script{
                   sh 'ansible-playbook -i inventory.yaml deployment_playbook.yaml -e "build_number=${BUILD_NUMBER}\"'
                }
                  
              }
        }*/
        
    }

}
