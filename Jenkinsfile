pipeline {
    agent {
        kubernetes {
        label "k8s-sdk-py-${cto.devops.jenkins.Utils.getTimestamp()}"
        inheritFrom 'k8s-proxy'
        yaml """
            spec:
            containers:
            - name: beluga
                image: sf-docker-releases.repo.lab.pl.alcatel-lucent.com/abllabs/beluga:latest
                workingDir: /home/jenkins
                tty: true
                command:
                - cat
        """
        }
    }
    triggers {
        gitlab(
            triggerOnPush: true,
            branchFilterType: 'All',
            triggerOnMergeRequest: true,
            triggerOpenMergeRequestOnPush: "never",
            triggerOnNoteRequest: true,
            triggerOnAcceptedMergeRequest: true,
            noteRegex: "Jenkins please retry a build",
            skipWorkInProgressMergeRequest: true,
            ciSkip: false,
            setBuildDescription: true,
            addNoteOnMergeRequest: true,
            addCiMessage: true,
            addVoteOnMergeRequest: true,
            acceptMergeRequestOnSuccess: true,
            cancelPendingBuildsOnUpdate: false,
        )
    }
    stages {
        stage('Test') {
            steps {
                container('beluga') {
                    script {
                        sh """
                        python3 -m poetry install
                        python3 -m poetry run pytest
                        """
                    }
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
