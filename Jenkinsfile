pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du projet GitHub...'
                checkout scm
            }
        }

        stage('Verification Node et Newman') {
            steps {
                bat 'node --version'
                bat 'npm --version'
                bat 'newman --version'
            }
        }

        stage('Execution des tests Postman') {
            steps {

                bat '''
                    newman run collection.json ^
                    --reporters cli,junit,json ^
                    --reporter-junit-export reports/junit-report.xml ^
                    --reporter-json-export reports/newman-report.json
                '''
            }
        }
    }

    post {

        always {

            echo 'Publication des rapports...'

            junit(
                testResults: 'reports/junit-report.xml',
                allowEmptyResults: true
            )

            archiveArtifacts(
                artifacts: 'reports/newman-report.json',
                allowEmptyArchive: true
            )
        }

        success {
            echo '======================================'
            echo '       TESTS POSTMAN : SUCCES'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo '       TESTS POSTMAN : ECHEC'
            echo '======================================'
        }
    }
}
