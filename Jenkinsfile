pipeline {
    agent none

    stages {

        stage('Checkout on Controller') {
            agent { label 'built-in' }
            steps {
                checkout scm

                echo "Running on Controller Node"
                echo "Workspace: ${WORKSPACE}"

                echo "Files present:"
                bat 'dir'
            }
        }

        stage('Process on Windows Agent') {
            agent { label 'win-agent-4' }
            steps {
                checkout scm

                echo "Running on Windows Agent"
                echo "Workspace: ${WORKSPACE}"

                echo "Copying index.html to output folder"
                bat '''
                    if exist out rmdir /s /q out
                    mkdir out
                    copy index.html out\\
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'out/**', fingerprint: true
        }
    }
}
