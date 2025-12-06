pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/yankoov/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Setup .net') {
            steps {
                script {
                    bat '''
                    echo Setting up .NET 8.0 SDK
                    choco install dotnet-8.0-sdk -y
                    '''
                }
            }
        }

        stage('Restore dependencies') {
            steps {
                script {
                    bat '''
                    echo Restoring .NET dependencies
                    dotnet restore
                    '''
                }
            }
        }

        stage('Build project') {
            steps {
                script {
                    bat '''
                    echo Building the .NET project
                    dotnet build
                    '''
                }
            }
        }

        stage('Run TestProject1') {
            steps {
                script {
                    bat '''
                    echo Running tests
                    dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=test_results.trx"
                    '''
                }
            }
        }

        stage('Run TestProject2') {
            steps {
                script {
                    bat '''
                    echo Running tests
                    dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=test_results.trx"
                    '''
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/test_results.trx', allowEmptyArchive: true

            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}
