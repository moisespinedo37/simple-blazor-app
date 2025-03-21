pipeline {
    agent any

    environment {
        DOTNET_ROOT = "C:\\Program Files\\dotnet"
        PATH = "C:\\Program Files\\dotnet;${env.PATH}"
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
                    def dotnetExists = bat(script: "dotnet --version", returnStatus: true)
                    if (dotnetExists != 0) {
                        sh 'winget install --id Microsoft.DotNet.SDK.9 -e --source winget'
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
