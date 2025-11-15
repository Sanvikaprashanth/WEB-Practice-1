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
                echo "Creating web directory in Jenkins workspace..."
                sh 'rm -rf ${WORKSPACE}/web'
                sh 'mkdir -p ${WORKSPACE}/web'
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
                docker '--version'
                sudo 'systemctl status docker'
                sudo 'yum install -y docker'
                sudo 'systemctl start docker'
                sudo 'systemctl enable docker'
                sudo 'usermod -aG docker jenkins'
                sudo 'su - jenkins'
                docker 'ps'

                sh '''
            docker rm -f apache1 || true
            docker run -dit --name apache1 -p 9000:80 -v ${WORKSPACE}/web:/usr/local/apache2/htdocs/ httpd
        '''
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application files...'
                sh 'cp -r ${WORKSPACE}/web/* ${WORKSPACE}/web/'
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
