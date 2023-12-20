pipeline {
    agent any


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

        stage('Test') {
            steps {
                // Run Django tests
                sh '''
                . .venv/bin/activate
                python3 manage.py makemigrations
                python3 manage.py migrate
                python manage.py collectstatic
                python3 manage.py test 
                '''
            }
        }

        stage('Build') {
            steps {
                // Additional build steps can be added here
                // For example, building a Docker image
                script {
                    docker.build("my-django-app")
                }
            }
        }

        // Additional stages like 'Deploy' can be added here
    }

    post {
        always {
            // Perform clean-up actions, if required
            echo 'Pipeline execution completed'
        }
    }
}
