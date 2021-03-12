pipeline {
    agent any
    
    tools  {
        maven "my-maven"
    }

    stages {
        // Start Sonarqube Server
        stage("Start Sonarqube Server"){
            steps{
                echo "====++++  Start Sonarqube Server ++++===="

                sh "sudo docker run -d --name my-sonarqube -p 9000:9000 sonarqube:lts"
            }          
        }

      // Clone from Git
        stage("Clone App from Git"){
            steps{
                echo "====++++  Clone App from Git ++++===="
                git branch:"master", url: "https://github.com/zakaria2905/challenge-09-CI-CD.git"
            }          
        }
        // Build and Unit Test (Maven/JUnit)
        stage("Build and Package"){
            steps{
                echo "====++++  Build and Unit Test (Maven/JUnit) ++++===="
                sh "mvn clean package"
            }           
        }

        // Static Code Analysis (SonarQube)        
        stage("Static Code Analysis (SonarQube)"){
            steps{
                echo "====++++  Static Code Analysis (SonarQube) ++++===="
                //withSonarQubeEnv('my_sonarqube_in_docker') {  
                sh "mvn clean package -Dsurefire.skip=true sonar:sonar -Dsonar.host.url=http://localhost:9000   -Dsonar.projectName=challenge-09-jenkins-ansible-playbook -Dsonar.projectKey=challenge-09-jenkins-ansible-playbook -Dsonar.projectVersion=$BUILD_NUMBER";
               }  
            }           

         // Deploiement du WAR sur le server-staging avec Ansible
        stage("Deploy WAR on staging using Ansible"){
            steps{
                
                echo "====++++  Deploy WAR on staging using Ansible ++++===="
       
               ansiblePlaybook   credentialsId: 'server-staging-ssh', 
                      //extras: '-e version=${VERSION}', 
                      //inventory: 'development', 
                      playbook: 'ansible/playbook-deploy-staging.yaml'             
            } 
        }

        // Stop Sonarqube Server
        stage("Stop Sonarqube Server"){
            steps{
                echo "====++++  Stop Sonarqube Server ++++===="
                sh "sudo docker stop my-sonarqube && sudo docker rm my-sonarqube"
            }          
        }        
    }
}