pipeline {
    agent any
    
    tools  {
        maven "my-maven"
    }

    stages {
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
    }
}
