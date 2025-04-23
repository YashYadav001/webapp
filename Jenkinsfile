pipeline {
    agent {
        label 'master'
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

        stage('Sonar-Report') {
            steps {
                withSonarQubeEnv('MySonar') {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=jenkins_project \
                            -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }
    }
}
