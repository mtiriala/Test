pipeline {
    agent any
    environment {
        registryCredential = 'docker'
        dockerImage = ''
        imageName = 'my-django-app' // Replace with your image name
        imageTag = 'v1.0' // Replace with your desired image tag
    }

    stages {
        stage('Preparation') {
            steps {
                checkout scm

                script {

 
                sh '''   

                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install -r requirements.txt
                    pip install django gunicorn
                '''
                    
                }

            }
        }

        //stage('Test') {
        //steps {
////////// Run Django tests
////////sh '''
////////. .venv/bin/activate
////////python3 manage.py makemigrations
////////python3 manage.py migrate
////////python manage.py collectstatic
////////python3 manage.py test 
               // '''
            //}
        //}



      stage('Build Docker Image') {
            steps {
                script {
                    // Building the Docker image with a tag using Docker Pipeline plugin
                    dockerImage = docker.build("${imageName}:${imageTag}")
                }
            }
        }
        stage('Run Docker Compose') {
            steps {
                script {
                    sh '''
                        docker-compose up -d --force-recreate
                    '''
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("${imageTag}")
                        // Optionally, push the 'latest' tag as well
                        dockerImage.push("latest")
                    }
                }
            }
        }

        // Additional stages like 'Deploy' can be added here
    }

}
