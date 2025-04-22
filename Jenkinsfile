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

        // Optional: Uncomment this if you want the Sonar report here
        // stage('Sonar-Report') {
        //     steps {
        //         sh '''
        //         mvn sonar:sonar \
        //             -Dsonar.projectKey=jenkins_project \
        //             -Dsonar.host.url=http://localhost:9000 \
        //             -Dsonar.login=5f09ded7e5db4d0ea0dcfd937c181af706e60475
        //         '''
        //     }
        // }

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
                sh '''
                mvn clean install sonar:sonar \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.analysis.mode=publish
                '''
            }
        }
    }
}
