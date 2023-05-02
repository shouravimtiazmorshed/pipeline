pipeline {
    agent any
    tools {
        maven 'Maven3.9.1'
        codeql 'CodeQL'
    }
    stages {
        stage('Maven build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('CodeQL Scan') {
            steps {
                withEnv(["PATH+CODEQL=D:\\DevOps\\codeql\\:$PATH"]) {
                    bat 'codeql database init --language=java --command="mvn clean package" --source-root=. --name=my-database --overwrite my-database'
                    bat 'codeql resolve languages'
                    bat 'codeql database finalize my-database'
                    bat 'codeql database analyze my-database --format=sarif-latest --output=results.sarif D:\\DevOps\\Reports\\pipeline.ql'
                }
            }
        }
        stage('Publish Results') {
            steps {
                withEnv(["PATH+CODEQL=D:\\DevOps\\codeql\\:$PATH"]) {
                    publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, reportDir: 'D:\\DevOps\\Reports\\', reportFiles: 'results.html', reportName: 'CodeQL Results'])
                    recordIssues(tools: [codeQL(pattern: '**/results.sarif')])
                }
            }
        }
    }
}
