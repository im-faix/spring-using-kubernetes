def version = 'v1.0'

pipeline
{
    agent any
<<<<<<< HEAD
=======
        environment {
        DOCKER_USER = 'faixan01'
        DOCKER_PASS = 'your_password'
    }
>>>>>>> 0ffb975 (update Jenkins file)
    
    stages
    {
        stage('clean-workspace')
        {
            steps
            {
                cleanWs()
            }
        }
	stage('stoping-container')
	{
		steps
		{
			sh 'docker stop spring-webapp || true'
		}
	}
        stage('cloning')
        {
            steps
            {
                sh 'git clone https://github.com/im-faix/Spring-Project-DevOps.git .'
                echo 'clone completed'
            }
        }
        stage('docker-image')
        {
            steps
            {
                sh "docker build -t webapp:$version ."
                echo "image created "
            }
        }
        stage('docker-container')
        {
            steps
            {
//                sh "docker run -d -p 9090:9090 webapp:$version"
                echo "docker image push to repository"
            }
        }
        stage('listing')
        {
            steps
            {
                sh 'docker images'
                echo ""
                sh 'docker ps '
		sh 'ls -lash'
            }
        }
	stage('Docker run')
	{
	    steps
	   {
		sh 'docker compose pull '
		sh 'docker compose up -d '
	   }
	}
		
    }
}
