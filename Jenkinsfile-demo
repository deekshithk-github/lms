pipeline {
    agent any
    stages {
        stage('Code Analysis') {
            steps {
               echo 'Sonar Analysis Started'
               sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://13.40.25.174:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_e4a1aace6c5c587f54529c25626c9beb526e5bdb" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
               echo 'Sonar Analysis Completed'
            }
        }
        stage('LMS Build') {
            steps {
               echo 'LMS Build Started'
               sh 'cd webapp && npm install && npm run build'
               echo 'LMS Build Completed'
            }
        }
        stage('Release LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://13.40.25.174:8081/repository/lms/"
                }
            }
        }
        stage('Deploy LMS - dev') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "curl -u admin:lms12345 -X GET \'http://13.40.25.174:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
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
