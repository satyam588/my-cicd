pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS-20' // Ensure NodeJS is set up in Jenkins Global Tool Configuration
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/satyam588/my-cicd.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        // stage('Test') {
        //     steps {
        //         sh 'npm test'
        //     }
        // }
        
        stage('Deploy') {
            steps {
                sshPublisher(
                    publishers: [sshPublisherDesc(
                        configName: 'Crazybooks.in',
                        transfers: [sshTransfer(
                            cleanRemote: false,
                            excludes: '',
                            // execCommand: '''
                            //     rm -rf /var/www/html/.next/*
                            //     cp -r .next/* /var/www/html/
                            //     cp -r public/* /var/www/html/public/
                            // ''',
                            execTimeout: 120000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: 'public_html',
                            remoteDirectorySDF: false,
                            removePrefix: 'build/',
                            sourceFiles: 'build/**/*'
                        )],
                        usePromotionTimestamp: false,
                        useWorkspaceInPromotion: false,
                        verbose: false
                    )]
                )
            }
        }
    }
}
