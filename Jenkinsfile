//Install Pipeline 
//Install Pipeline Utility Steps 
//Install Workspace Cleanup
pipeline {    
    agent any 
    stages {
        stage('Code Quality') { 
            steps {
               echo 'Sonar Analysis Started'
               sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://20.2.139.83:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_69bdb243cd1da6eb01895d5f00258246b79ce06f" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
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
                   sh "curl -v -u admin:Milky@123456 --upload-file webapp/lms-${packageJSONVersion}.zip http://20.2.139.83:8081/repository/lms-project/"
               }
            }
        }
        stage('Deploy LMS') {
           steps {
               script {
                   def packageJson = readJSON file: 'webapp/package.json'
                   def packageJSONVersion = packageJson.version
                   echo "${packageJSONVersion}"
                   sh "curl -u admin:Milky@123456 -X GET \'http://20.2.139.83:8081/repository/lms-project/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
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