pipeline {
    agent any
    environment {
        VENV_PATH = "venv"
        EMAIL_TO = "maria.donner@skyered-education.de"
    }
    stages {
        stage('Build') {
            steps {
                sh 'python3 -m venv $VENV_PATH'
                sh './$VENV_PATH/bin/pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                sh './$VENV_PATH/bin/pytest tests/ --junitxml=test-results.xml'
            }
        }
        stage('Artefakt speichern') {
           steps {
                sh 'zip -r app.zip .'
                archiveArtifacts artifacts: 'app.zip', fingerprint: true
            }
        }

    }

    post {
        success {
            echo "Build Success"
        }

        failure {
            emailext (
                subject: "Pipeline fehlgeschlagen: ${currentBuild.fullDisplayName}",
                body: "Die Pipeline ist fehlgeschlagen. Bitte überprüfen Sie die Build-Logs unter: ${env.BUILD_URL}",
                to: "${EMAIL_TO}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}