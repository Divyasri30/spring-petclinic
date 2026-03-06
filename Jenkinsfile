pipeline {
    agent { label 'spc' }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/Divyasri30/spring-petclinic.git',
                    branch: 'main'
            }
        }

        stage('Build and Sonar Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sonar') {
                        sh """
                        mvn clean package sonar:sonar \
                        -Dsonar.projectKey=divyasri30 \
                        -Dsonar.organization=divyasri30 \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }

        stage('Upload Artifact to JFrog') {
            steps {
                rtUpload(
                    serverId: 'JFROG',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "spcjava-spc/"
                            }
                        ]
                    }'''
                )

                rtPublishBuildInfo(serverId: 'JFROG')
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.jar'
            junit '**/surefire-reports/*.xml'
        }
    }
}