pipeline {
    agent none

    stages {
        stage('Checkout on Controller') {
            agent { label 'built-in' }
            steps {
                checkout scm
                echo "Running on Controller"
                bat 'dir'
            }
        }

        stage('Process on Windows Agent') {
            agent { label 'win-agent-4' }
            steps {
                checkout scm
                bat """
                    if exist out rmdir /s /q out
                    mkdir out
                    copy index.html out\\
                """
            }
        }
    }

    post {
        always {
            node('win-agent-4') {   // 👈 FIX: Run post inside a node
                archiveArtifacts artifacts: 'out/**', fingerprint: true
            }
        }
    }
}
