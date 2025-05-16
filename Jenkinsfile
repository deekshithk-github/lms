//Install Pipeline 
//Install Pipeline Utility Steps 
//Install Workspace Cleanup
pipeline {    
    agent any 
    stages {
        stage('Code Quality') { 
            steps {
               echo 'Sonar Analysis Started'
               sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://20.246.60.11:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_5f8eb0ae1b29061559356150ab8c219001df5a90" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
               echo 'Sonar Analysis Completed'
            }
        }
        stage('Build LMS') { 
            steps {
               echo 'LMS Build Started'
               sh 'cd webapp && npm install && npm run build'
               echo 'LMS Build Completed' 
            }
        }
        stage('Publish LMS NEXUSDeploy') { 
            steps {
                script {
                   def packageJson = readJSON file: 'webapp/package.json'
                   def packageJSONVersion = packageJson.version
                   echo "${packageJSONVersion}"
                   sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                   sh "curl -v -u admin:Milky@123456 --upload-file webapp/lms-${packageJSONVersion}.zip http://20.246.60.11:8081/repository/lms/"
               }
            }
        }
        stage('Deploy LMS') {
           steps {
               script {
                   def packageJson = readJSON file: 'webapp/package.json'
                   def packageJSONVersion = packageJson.version
                   echo "${packageJSONVersion}"
                   sh "curl -u admin:Milky@123456 -X GET \'http://20.246.60.11:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                   sh 'sudo rm -rf /var/www/html/*'
                   sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                   sh "sudo cp -r webapp/dist/* /var/www/html"
               }
           }
       }

       stage('Clean Up Workspace') {
           steps {
                   echo 'Cleaning Work Space'
                   // Install Cleanup Workspace plugin to make below command work
                   cleanWs()
           }
       }
    }
}
