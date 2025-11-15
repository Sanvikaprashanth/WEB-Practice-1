pipeline {
    agent any
   
    stages {

        stage('Input Parameters') {
            input {
                message 'Enter deployment details'
                parameters {
                    string(name: 'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment')
                    string(name: 'ENVIRONMENT', defaultValue: 'Development', description: 'Environment to deploy')
                }
            }
            steps {
                echo "The responsible of this project is ${params.AUTHOR} and will be deployed in ${params.ENVIRONMENT}"
            }
        }

        stage('Create web directory') {
            steps {
                echo 'Creating /home/jenkins/web directory...'
                sudo 'mkdir -p /home/jenkins/web'
                sudo 'chown -R jenkins:jenkins /home/jenkins/web'
                sudo 'chmod -R 755 /home/jenkins/web'

            }
        }

        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'Dropping the existing container if any...'
                sh 'docker rm -f apache1 || true'
            }
        }

        stage('Create the Apache HTTPD container') {
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name apache1 -p 9000:80 -v /home/jenkins/web:/usr/local/apache2/htdocs/ httpd'
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application files...'
                sh 'cp -r ${WORKSPACE}/web/* /home/jenkins/web/'
            }
        }

        stage('Checking the app') {
            steps {
                echo 'Testing the web application...'
                sh 'wget -qO- http://localhost:9000'
            }
        }

    }
}
