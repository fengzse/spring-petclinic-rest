pipeline{
    agent any
    stages{
        stage('Para'){
            parallel{

                 stage('Build and Run the Server'){
                            steps{
                                sh 'cd spring-petclinic-rest && nohup mvn spring-boot:run &'
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
                                    sh 'cd spring-petclinic-angular/static-content && curl https://jcenter.bintray.com/com/athaydes/rawhttp/rawhttp-cli/1.0/rawhttp-cli-1.0-all.jar -o rawhttp.jar && nohup java -jar ./rawhttp.jar serve . -p 4200 &'
                              }
                }
                stage('Robot Framework') {
                              steps {
                                    sleep(70)
                                    sh 'robot --variable BROWSER:headlesschrome -d RobotFrameWork/Results RobotFrameWork/Tests'
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
    }
}
