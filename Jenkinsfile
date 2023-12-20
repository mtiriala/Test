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
                    chmod +777 "$(pwd)/venv/bin/activate"
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
                    // Building the Docker image with a tag
                sh 'docker build -t my-django-app:v1.0 .'
                }
            }
        }
      stage('Build Docker Image') {
            steps {
                script {
                    // Building the Docker image with a tag using Docker Pipeline plugin
                    dockerImage = docker.build("${imageName}:${imageTag}")
                }
            }
        }

        // Additional stages like 'Deploy' can be added here
    }

}
