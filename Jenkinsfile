pipeline
{
    agent any
    environment
    {
        DOCKER_IMAGE_NAME = "abellimwk/train-schedule"
    }
    
    stages
    {
        stage('Build')
        {
            steps
            {
                echo 'Running Build Automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
        stage('Build Docker Image')
        {
            when
            {
                branch 'master'
            }
            
            steps
            {
                script
                {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_key')
                    {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
