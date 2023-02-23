pipeline {
    agent any
    options {
        timestamps()
        ansiColor('xterm')
    }
    stages{
        stage('Imagen'){
            steps{
                sh '''
		        docker-compose build
                git tag 1.0.${BUILD_NUMBER}
                docker tag ghcr.io/frangel13v/hello-springrest/hello-springrest:latest ghcr.io/frangel13v/hello-springrest/springrest:1.0.${BUILD_NUMB
                '''
                sshagent(['ssh-github']) {
                    sh('git push git@github.com:frangel13v/hello-springrest.git --tags')
                }
	    }	                              
        }  
        
        stage('Subir Imagen'){
            steps{ 
		withCredentials([string(credentialsId: 'github-token', variable: 'GIT_TOKEN')]){
		    sh 'echo $GIT_TOKEN | docker login ghcr.io -u frangel13v --password-stdin'
                    sh 'docker push ghcr.io/frangel13v/hello-springrest/springrest:1.0.${BUILD_NUMBER}'
		}
            }
        }
/*        stage('eb Deploy'){
            steps{
                withAWS(credentials: 'angelfr-aws-credentials') {
                    dir(app){
                        sh 'eb deploy hello-springrest-dev --version 1.0.${BUILD_NUMBER}'
                    }
                }
          } 
        }*/
/*
        stage('Jacoco'){
            steps{
                jacoco (
                    classPattern: './app/',
                    execPattern: './app/gradlew',
                    runAlways: true,
                    sourcePattern: './app/'
                    )
            }
        }
*/
        stage('Jacoco'){
            steps{
                jacoco (
                    execPattern: './build/jacoco/*.exec',
                    runAlways: true
                    )
            }
        }
    }
} 
