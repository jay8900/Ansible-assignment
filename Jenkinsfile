pipeline {
    // ① Select a Jenkins slave with Docker capabilities
    agent {
        label 'docker'
    }

    environment {
        PRODUCT = 'ghcli'
        GIT_HOST = 'somewhere'
        GIT_REPO = 'repo'
    }

    options {
        ansiColor('xterm')
        skipDefaultCheckout()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        // ② Checkout the right branch
        stage('Checkout') {
            steps {
                script {
                    BRANCH_NAME = env.CHANGE_BRANCH ? env.CHANGE_BRANCH : env.BRANCH_NAME
                    deleteDir()
                    git url: "git@<host>:<org>/${env.PRODUCT}.git", branch: BRANCH_NAME
                }
            }
        }

	// ③ Build a container with the code source of the application
        stage('Build') {
            steps {
                sh "docker build . -t ${env.PRODUCT}:py"
            }
        }
