pipeline {
    agent any
   
    stage('Create web directory') {
    input {
        message 'Enter the data'
        parameters {
            string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment ')
            string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
        }
    }
    steps {
        echo "The responsible of this project is ${params.AUTHOR} and will be deployed in ${params.ENVIRONMENT}"
        sh 'rm -rf /home/jenkins/web'
        sh 'mkdir -p /home/jenkins/web'

                
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name apache1 -p 9000:80  -v /home/jenkins/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                sh 'cp -r ${WORKSPACE}/web/* /home/jenkins/web/'

            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9000'
            }
        }       
    }
}
