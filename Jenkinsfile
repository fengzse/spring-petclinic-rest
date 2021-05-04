pipeline{
    agent any
    stages{
        stage('Para'){
            parallel{
                 stage('Checkout the Server'){
                     steps{
                         git 'https://github.com/fengzse/spring-petclinic-rest.git'
                     }
                }
                 stage('Build and Run the Server'){
                            steps{
                                sh "mvnw.cmd spring-boot:run"
                            }
                }
                 stage('Checkout the Angular'){
                            steps{
                                git 'https://github.com/fengzse/spring-petclinic-angular.git'
                            }
                 }
                 stage('Run the Frontend'){
                              steps{
                                    sleep(60)
                                    sh 'curl https://jcenter.bintray.com/com/athaydes/rawhttp/rawhttp-cli/1.0/rawhttp-cli-1.0-all.jar -o rawhttp.jar'
                                    sh 'java -jar ./rawhttp.jar serve . -p 4200'
                              }
                }
                stage('Robot Framework System tests with Selenium') {
                              steps {
                                    sleep(70)
                                    sh 'robot --variable BROWSER:headlesschrome -d RobotFrameWork/Results  RobotFrameWork/Tests'
                              }
                              post {
                                    always {
                                           script {
                                                  step(
                                                       [
                                                             $class              : 'RobotPublisher',
                                                              outputPath          : 'RobotFrameWork/Results',
                                                              outputFileName      : '**/output.xml',
                                                              reportFileName      : '**/report.html',
                                                              logFileName         : '**/log.html',
                                                              disableArchiveOutput: false,
                                                              passThreshold       : 50,
                                                              unstableThreshold   : 40,
                                                              otherFiles          : "**/*.png,**/*.jpg",
                                                       ]
                                                  )
                                           }
                                    }
                              }
                }
            }
       }

        stage('newman') {
              steps {
                     sh 'newman run RestfulBooker_Feng/Restful_Booker_Feng_LabPostman.postman_collection.json --environment RestfulBooker_Feng/Restful_Booker_Feng.postman_environment.json --reporters junit'
              }
              post {
                    always {
                         junit '**/*xml'
                        }
              }
        }
    }
}
