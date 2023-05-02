pipeline {
    agent any
    tools {
        maven 'Maven3.9.1'
        codeql 'CodeQL2.13.1'
    }
    stages {
        stage('Maven build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('CodeQL Scan') {
            steps {
               
                    bat 'codeql database init --language=java --command "mvn clean package" --source-root=. --name=my-database --overwrite'
                    bat 'codeql resolve languages'
                    bat 'codeql database finalize my-database'
                    bat 'codeql database analyze my-database --format=sarif-latest --output=results.sarif D:\\DevOps\\Reports\\pipeline.ql'
                
            }
        }
        stage('Publish Results') {
            steps {
               
                    publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, reportDir: 'D:\\DevOps\\Reports\\', reportFiles: 'results.html', reportName: 'CodeQL Results'])
                    recordIssues(tools: [codeQL(pattern: '**/results.sarif')])
                
            }
        }
    }
}
