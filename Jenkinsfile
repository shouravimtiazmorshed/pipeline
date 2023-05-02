
pipeline {
    agent any
	tools {
        maven 'Maven3.9.1' 
         // Define CodeQL as a tool installation
        codeql 'CodeQL'
    }
    
    environment {
        // Define scannerHome as an environment variable using the CodeQL tool
        scannerHome = tool 'CodeQL'
        // Define queriesDir as an environment variable
        queriesDir = 'your-codeql-queries-dir'
        // Define database as an environment variable
        database = 'your-codeql-database'
        // Define language as an environment variable
        language = 'your-codeql-language'
    }
    stages {
  
    stage('CodeQL Scan') {
            steps {
                // Use the CodeQL tool to run a scan
                bat 'D:\\DevOps\\codeql\\codeql database analyze --language=${language} --format=sarif-latest --output=results.sarif ${database} ${queriesDir}'
            }
        }
        stage('Publish Results') {
            steps {
                // Publish the CodeQL results in Jenkins
                recordIssues(
                    tool: codeql(
                        pattern: 'results.sarif'
                    )
                )
            }
        }
        stage('Maven build') {
            steps {
                 bat 'mvn -B -DskipTests clean package' 
            }
        }
    }
}