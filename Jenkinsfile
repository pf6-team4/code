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
        stage("Docker version"){
            steps{
                sh "docker version"
            }
        }
        stage("Docker Building Stage"){
            steps{ 
                sh "docker build -t custom-jar-image ."   
	        sh "docker tag custom-jar-image pf6team4/custom-jar-image"
            }
        }
        stage("push to dockerhub"){
            steps{ 
		withCredentials([string(credentialsId: 'dockerhub', variable: 'Dockerhubpwd')]) {
		    sh "docker login -u pf6team4 -p ${Dockerhubpwd}"
		}
	        sh "docker push  pf6team4/custom-jar-image"   
             }
        }
	stage ("Deploy docker"){
	     steps{
		ansiblePlaybook credentialsId: 'Dev_server', disableHostKeyChecking: true, inventory: 'dev.inv', playbook: 'docker_app_depluy.yml'
	     }
	}
    }
    post{
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
    }   
}
