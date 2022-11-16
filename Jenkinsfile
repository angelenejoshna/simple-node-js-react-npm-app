pipeline {
    agent {
        docker {
            // Runs this image as a separate container
            // You’ll have separate Jenkins and Node containers running locally in Docker
            // The Node container becomes the agent that Jenkins uses to run your Pipeline project
            image 'node:lts-bullseye-slim'

            // Makes the Node container (temporarily) accessible through port 3000
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                // Runs a shell script which executes the npm command to install dependencies
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                 sh 'chmod +x ./jenkins/scripts/test.sh'
                // Runs a shell testing script
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                 sh 'chmod +x ./jenkins/scripts/deliver.sh'
                // Runs a shell delivery script
                sh './jenkins/scripts/deliver.sh'

                // Pauses the running build and prompts the user (with a custom message) to proceed or abort.
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                 sh 'chmod +x ./jenkins/scripts/kill.sh'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
