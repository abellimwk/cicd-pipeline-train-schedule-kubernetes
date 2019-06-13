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
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside
                    {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        
        stage('Push Docker Image')
        {
            when
            {
                branch 'master'
            }
            
            steps
            {
                script
                {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
                    {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        
        stage('Deploy To Production')
        {
            when
            {
                branch 'master'
            }
            
            steps
            {
                input 'Deploy to production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
