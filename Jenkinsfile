pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Archive') {
            steps {
                archive 'target/*.jar'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('PostBuild') {
            steps {
                post {
                      always {
                        sh 'echo "Execution completed"'
                      }
                      success {
                        sh 'echo "Build Success"'
                      }
                      failure {
                        sh 'echo "Build Failed"'
                      }
                      unstable {
                        sh 'echo "The project is unstable"'
                      }
                      changed {
                        sh 'echo "The state of the Pipeline has changed"'
                      }
                }
             }
        }
    }

}

