
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
                    
                    bat 'codeql database init --language=java --source-root=. --overwrite my-database '
                    

        // Install any required dependencies for the CodeQL queries
        bat 'codeql resolve languages'

        // Run the CodeQL analysis
        bat 'codeql analyze --database=my-database  --sarif-path=results.sarif D:\\DevOps\\Reports\\pipeline.ql'
      
                
                }
            }
        }
 
        
    }
}