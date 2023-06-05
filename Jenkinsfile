pipeline {
    agent any

    stages {
        stage('clean') {
            steps {
                echo 'Clean up'
                bat 'mvn clean'
            }
        }
        stage('Build') {
            steps {
                echo 'Executing Build'
                bat 'mvn compile'
            }
        }
         stage('Test') {
            steps {
                echo 'Test '
                bat 'mvn test'
            }
        }
         stage('Package') {
            steps {
                echo 'Package'
                bat 'mvn package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                    bat 'start /b java -jar -Dserver.port=8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar'
                }
            }
        }
        
    }
}
