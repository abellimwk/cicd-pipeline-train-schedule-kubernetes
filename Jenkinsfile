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
                archiveArtifacts artifacts: 'dist/train-schedule.zip'
            }
        }
    }
}
