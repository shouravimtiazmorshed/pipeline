
pipeline {
    agent any
	tools {
        maven 'Maven3.9.1' 
         // Define CodeQL as a tool installation
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
               // Set up CodeQL environment variables
                withEnv(["PATH+CODEQL=D:\\DevOps\\codeql\\:$PATH"]) {
                    
                    bat 'codeql database init --language=java --command='mvn clean install -DskipTests' --source-root=. --overwrite my-database '
                    

        // Install any required dependencies for the CodeQL queries
        bat 'codeql resolve languages'

		// finalize database
      bat 'codeql database finalize my-database'
        // Run the CodeQL analysis
        bat 'codeql database analyze my-database --format=sarif-latest --output=results.sarif D:\\DevOps\\Reports\\pipeline.ql'
      
                
                }
            }
        }
  stage('Publish Results') {
  steps {
               // Set up CodeQL environment variables
                withEnv(["PATH+CODEQL=D:\\DevOps\\codeql\\:$PATH"]) {
                
        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, reportDir: 'D:\\DevOps\\Reports\\', reportFiles: 'results.html', reportName: 'CodeQL Results'])
        recordIssues(tools: [codeQL(pattern: '**/results.sarif')])
        }
        }
    }
        
    }
}