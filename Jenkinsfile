
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
                    // Add the CodeQL query repository as a Git submodule
        bat 'git submodule add https://github.com/github/codeql-java.git'

        // Install any required dependencies for the CodeQL queries
        bat 'codeql resolve languages'

        // Run the CodeQL analysis
        bat 'codeql analyze --database=my-database --sarif-path=results.sarif codeql-java/java-security-extended.qls'
      
                
                }
            }
        }
 
        
    }
}