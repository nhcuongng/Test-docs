pipeline {
    agent any

    environment {
        NPM_RELEASE = 'https://artifact.nguoidilam.com/api/npm/npm-release/'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }

    stages {
        stage('Checkout code') {
            steps {
                sh 'printenv'
                checkout scm
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                withNPM(npmrcConfig: 'artifactory-npm') {
                    sh '''
                        npm version 1.0.${BUILD_NUMBER} --no-git-tag-version;
                        yarn;
                        yarn generate;
                        yarn build;
                        npm publish --registry ${NPM_RELEASE};
                    '''
                }
            }
        }
    }
}