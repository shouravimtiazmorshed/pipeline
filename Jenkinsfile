
pipeline {
    agent any


    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'your-git-credentials-id', url: 'your-git-repo-url']]])
            }
        }
        stage('CodeQL scan') {
            steps {
                withCodeql() {
                    def scannerHome = tool 'CodeQL'
                    def queriesDir = 'your-codeql-queries-dir'
                    def database = 'your-codeql-database'
                    def language = 'your-codeql-language'

                    codeqlCli(cmd: "database analyze --output sarif --sarif-category-property impact -o results.sarif --format-summary -s ${database} ${scannerHome}/bin/codeql ${scannerHome}/codeql-home/semmle/codeql-java-queries/java ${language} ${queriesDir}")
                }
            }
        }
        stage('Maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}