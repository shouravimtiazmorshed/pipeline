pipeline {
     agent any
    tools {
        maven 'Maven3.9.1' 
    }
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
    stages {
        stage('Build') { 
        
            steps {
                bat 'mvn -B -DskipTests clean package' 
            }
        }
        
      stage('CodeQL Scan') {
            steps {
                //withEnv(['JAVA_HOME=C:\\Program Files\\Java\\jdk1.8.0_281', 'PATH+MAVEN=C:\\apache-maven-3.8.3\\bin']) {
               // withEnv(){
                    
                

                    bat 'codeql database create jenkins-database --language=java --source-root=.\\ --exclude=.\\**\\target\\** --verbose'
                    bat 'codeql database analyze jenkins-database jenkins-codeql-results --format=sarif-latest --verbose'
                //}
            }
        }
        
        stage('Publish Results') {
            steps {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, includes: '**/codeql-results.*/codeql-scan.sarif', keepAll: true, reportDir: 'F:\\Shourav\\Reports', reportFiles: 'codeql-results.html', reportName: 'CodeQL Results'])
                archiveArtifacts artifacts: 'codeql-results.*'
            }
        }
         stage('Copy'){
      		steps {
               
          		bat ("copy target\\*.jar F:\\Shourav")
        		
      			}
    	}
    	
    	 stage('TestRun') {
            steps {
            	//def command = "cmd /c start F:\\Shourav\\Java\\bin\\javaw -jar F:\\Shourav\\gateway-0.0.1.jar"
            	//command.execute()
            	 bat ("cmd /c start /b F:\\Shourav\\Java\\bin\\javaw -jar F:\\Shourav\\gateway.jar >> F:\\Shourav\\JenkinsCmd.txt 2>&1")
            	// bat ("cmd /c start  F:\\Shourav\\Java\\bin\\javaw -jar F:\\Shourav\\gateway.jar ")
               //bat ("F:\\Shourav\\Java\\bin\\javaw -jar F:\\Shourav\\gateway.jar")
            }
        }
        
        stage('Testing') {
            steps {
                bat ("F:\\Shourav\\nodejs\\newman run F:\\Shourav\\Jenkins_test_postman_collection.json  --disable-unicode  --reporters cli,json --reporter-json-export  F:\\Shourav\\JenkinsTestReport.json")
            }
        }
    
    }
}