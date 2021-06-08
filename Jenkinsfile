pipeline{
    agent any
    tools{
        maven "maven-3.6.1"
    }
    stages{
        stage("Checkout Stage"){
            steps{
                git branch: 'production',
                credentialsId: 'git',
                url: 'https://github.com/pf6-team4/code.git'
            }
        }
        stage("Compilation Stage"){
            steps{
                sh "mvn compile"
            }
        }
        stage("Testing Stage"){
            steps{
                sh "mvn test"
            }
            post{
                always{
                    junit '**/target/surefire-reports/*.xml'    
                }
                failure{
                    echo "Failed some of the tests"
                }
            }
        }
        stage("Package Stage"){
            steps{
                sh "mvn package"
            }
        }
        // stage("Docker version"){
        //     steps{
        //         sh "docker version"
        //     }
        // }
      /*  stage("Docker Building Stage"){
            steps{ 
                sh "docker build -t custom-jar-image ."   
            }
        }
        stage("Docker Running Stage"){
            steps{
                sh "docker run -d -p 5555:5555 custom-jar-image"
            }
        }*/
    }
    /*post{
        success{
            mail to: "kskouwp@gmail.com, pf6.team4@gmail.com",
            subject: "Jenkins - Successful Build : $BUILD_TAG",
            body: "Go check it out at $BUILD_URL"
        }
        failure{
            mail to: "kskouwp@gmail.com, pf6.team4@gmail.com",
            subject: "Jenkins - Unsuccessful Build : $BUILD_TAG",
            body: "Go check it out at $BUILD_URL"
        }
    }  */  
}
