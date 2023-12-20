pipeline {
    agent any

    environment {
        VENV = 'venv'
    }

    stages {
        stage('Preparation') {
            steps {
                checkout scm

                script {
                    if (!fileExists(VENV)) {
                        sh''' 
                          pip3 -m install virtualenv
                          python3 -m venv $VENV
                        '''


                    }
                }

                // Install dependencies
                sh '''
                    cd "$VENV/"
                    ls
                    python3 -m pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                // Run Django tests
                sh '''
                    . $VENV/bin/activate
                    python manage.py test
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
