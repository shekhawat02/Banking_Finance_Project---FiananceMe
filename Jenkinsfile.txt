 pipeline {
    agent any
    stages{
        stage('Build Project'){
            steps{
                // 1. Checkout the project code
                git url:'https://github.com/shekhawat02/Banking_Finance_Project---FiananceMe.git', branch: "main"
                
                // 2. Build the Maven project
                sh 'mvn clean package'
            }
        }
        
        stage('Build Docker Image'){
            steps{
                script{
                    // 3. Build the Docker image
                    sh 'docker build -t shekhawat02/staragileprojectfinance:v1 .'
                    
                    // 4. (Optional) Verify the image was built
                    sh 'docker images | grep shekhawat02/staragileprojectfinance'
                }
            }
        }
          
        stage('Deploy') {
            steps {
                script {
                    def containerName = 'finance-app-deployment'
                    def imageTag = 'shekhawat02/staragileprojectfinance:v1'
                    
                    // Best Practice: Stop and remove any existing container with the same name before deployment
                    // This prevents the pipeline from failing on subsequent runs.
                    echo "Checking for and stopping existing container: ${containerName}"
                    sh "docker stop ${containerName} || true" // '|| true' ensures the step doesn't fail if the container doesn't exist
                    sh "docker rm ${containerName} || true"
                    
                    // 5. Deploy the application using Docker run
                    // MISTAKE FIX: 'itd' is changed to '-d' (detached mode)
                    // NOTE: Running docker without 'sudo' is preferred; ensure Jenkins user is in the 'docker' group.
                    echo "Starting new container: ${containerName}"
                    sh "docker run -d --name ${containerName} -p 8083:8081 ${imageTag}"
                }
            }
        }
    }
}
