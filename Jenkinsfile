pipeline{
    agent{
  label 'buildserver'
    }
     tools{
        maven 'mymaven'
     }
      stages{
        stage("build"){
            steps{
                script{
                    sh 'mvn install -Dv=${BUILD_NUMBER}'
                }

            }
        }
        /*stage("unit testing"){
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
        }*/
        /*stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'mysonar') {
                        sh "${tool("mysonar")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.27.2:9000 \
                        -Dsonar.login=sqp_25f7dde585f6341f3a80f67cb9affbd0631c196e"
    
                    }
                }
            }

        }*/
        stage("upload artifact"){
            steps{
               sh 'mvn -s settings.xml deploy -Dv=${BUILD_NUMBER} -DuniqueVersion=false'
            }
        }
        stage("deployment"){
            agent{
                label 'ansible_master'
            }
              steps{
                script{
                   sh 'ansible-playbook -i inventory.yaml deployment_playbook.yaml -e "build_number=${BUILD_NUMBER}\"'
                }
                  
              }
        }
        
    }

}