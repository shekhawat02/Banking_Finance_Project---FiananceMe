pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/shekhawat02/Banking_Finance_Project---FiananceMe.git', branch: "main"
                sh 'mvn clean package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shekhawat02/staragileprojectfinance:v1 .'
                    sh 'docker images'
                }
            }
        }
         
        
     stage('Deploy') {
            steps {
                sh 'sudo docker run -d -itd --name My-first-containe2510 -p 8083:8081 shekhawat02/staragileprojectfinance:v1'
                  
                }
            }
        
    }
}
