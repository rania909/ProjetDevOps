pipeline {


    agent any
    stages{
        stage('Checkout GIT'){
         steps {
             echo 'Pulling ...';
              git branch: 'hamza',
              url : 'https://github.com/hajerhassine/ProjetDevOps.git'
         } 

        }
        stage('Building image docker-compose') {
          steps {

              sh "docker-compose up -d"
          }
        }

         stage('Build image') {
          steps {
            sh "docker build -t lassoued404/imagedevops ."
       		}
       		}
    		
 	   stage('Push image') {
 		steps {
 	       withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
 			
        	 sh "docker push lassoued404/imagedevops"
        	}
        	}
        	}
	   stage('Run project') {
 		steps {
 	        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
 			
        	   sh "docker container run it lassoued404/imagedevopse /bin/sh"
        	}
        	}
        	}
         stage('Cleaning up') {
 		steps {
 	       withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
 			
        	 sh "docker rmi -f lassoued404/imagedevops"
        	}
        	}
        	}


        stage('Testing maven'){
            steps {
                sh """mvn -version """
            }

        }
        stage('MVN CLEAN'){
            steps {
                sh 'mvn clean'
            }
        }
        stage('MVN COMPILE'){
            steps {
                sh 'mvn compile'
            }
        }
		stage('SonarQube analysis 1') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=lassoued'
            }
        }
        

          stage('MVN Nexus'){
            steps {
                sh 'mvn redeploy'
            } 
            }         

        stage('JUnit and Mockito Test'){
            steps{
                script
                {
                    if (isUnix())
                    {
                        sh 'mvn --batch-mode test'
                    }
                    else
                    {
                        bat 'mvn --batch-mode test'
                    }
                }
            }
        }



    }
}