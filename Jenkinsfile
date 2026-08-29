pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '1',
                    usernameVariable: 'jenkins-banking-application',
                    passwordVariable: 'harika@123'
                )]) {

                    sh '''
                        mvn deploy:deploy-file \
                        -DgroupId=com.example \
                        -DartifactId=demo \
                        -Dversion=0.0.1-SNAPSHOT \
                        -Dpackaging=jar \
                        -Dfile=target/demo-0.0.1-SNAPSHOT.jar \
                        -DrepositoryId=nexus \
                        -Durl=http://172.31.30.140:8081/repository/Banking-application-snapshot/ \
                        -Dusername=$NEXUS_USERNAME \
                        -Dpassword=$NEXUS_PASSWORD
                    '''
                }
            }
        }
    }
}


