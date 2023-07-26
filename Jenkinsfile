pipeline {
    agent {
        docker {
            image 'sf-docker-releases.repo.lab.pl.alcatel-lucent.com/abllabs/beluga:latest'
        }
    }
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Test') {
            steps {
                script {
                        sh """
                        python3 -m poetry install
                        python3 -m poetry run pytest
                        """
                }  
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}
