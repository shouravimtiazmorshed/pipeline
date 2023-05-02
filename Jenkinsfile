
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
  
        stage('CodeQL scan') {
            steps {
                withCodeql() {
                    

                    codeqlCli(cmd: "database analyze --output sarif --sarif-category-property impact -o results.sarif --format-summary -s ${database} ${scannerHome}/bin/codeql ${scannerHome}/codeql-home/semmle/codeql-java-queries/java ${language} ${queriesDir}")
                }
            }
        }
        stage('Maven build') {
            steps {
                 bat 'mvn -B -DskipTests clean package' 
            }
        }
    }
}