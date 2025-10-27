pipeline {
    agent any
    options{
	skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                bat 'dotnet restore'
                bat 'dotnet build --no-restore'
            }
        }
        stage('Test') { 
            steps {
                bat 'dotnet test --no-build --no-restore --collect "XPlat Code Coverage"' 
            }
            post {
                always {
                    recordCoverage(tools: [[parser: 'COBERTURA', pattern: '**/*.xml']], sourceDirectories: [[path: 'SimpleWebApi.Test/TestResults']])  
                }
            }
        }
	stage('Deliver') {
            steps {
                bat 'dotnet publish SimpleWebApi --no-restore -o published'
            }
            post {
                success {
                    archiveArtifacts 'published/*.*'
                }
            }
        }
    }
}