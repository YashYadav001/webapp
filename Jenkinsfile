pipeline {
    agent {
        label 'master'
    }

    tools {
        maven 'Maven 3'
    }

    environment {
        PROJECT_VERSION = "1.0.0"
        GROUP_ID = "com.example"
        ARTIFACT_ID = "jenkins-demo"
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
                // This uses the credentials and server config set under 'MySonar' in Jenkins → Manage Jenkins → Configure System → SonarQube servers
                withSonarQubeEnv('MySonar') {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=jenkins_project \
                            -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        curl -v -u $USERNAME:$PASSWORD \
                        --upload-file target/${ARTIFACT_ID}-${PROJECT_VERSION}.jar \
                        http://localhost:8081/repository/maven-releases/$(echo ${GROUP_ID} | tr '.' '/')/${ARTIFACT_ID}/${PROJECT_VERSION}/${ARTIFACT_ID}-${PROJECT_VERSION}.jar
                    '''
                }
            }
        }
    }
}
