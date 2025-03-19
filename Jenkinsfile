pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${env.HOME}/.dotnet"
        PATH = "${env.HOME}/.dotnet:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/joedayz/simple-blazor-app.git'
            }
        }

        stage('Install .NET 9') {
            steps {
                script {
                    def dotnetExists = sh(script: "dotnet --version || echo 'not_installed'", returnStdout: true).trim()
                    if (dotnetExists == 'not_installed') {
                        sh 'wget https://download.visualstudio.microsoft.com/download/pr/dotnet-install.sh -O dotnet-install.sh'
                        sh 'chmod +x dotnet-install.sh'
                        sh './dotnet-install.sh --channel 9.0'
                    }
                }
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o output'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'output/**', fingerprint: true
            }
        }
    }
}
