pipeline {
    agent {
        label 'master'
    }
    environment {
        // Optional if you want to reuse project key or set any other global values
        SONAR_PROJECT_KEY = 'jenkins_project'
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
                withSonarQubeEnv('sqa_7a96cc273050392e2fc873ca031ab499020fad55') {
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
