pipeline {
    agent{label 'spec'}
    triggers{
        pollSCM('* * * * *')
    }
    stages {
        stage ('git checkout'){
            steps {
                git url:'https://github.com/Divyasri30/spring-petclinic.git',
                    branch:'main'
            }
        }
        stage ('build and scan'){
            steps{
                 withCredentials([string(credentialsId:'sonar_id', variable:'sonar')]){
                    withSonarQubeEnv('SONAR'){
                        sh """ mvn package sonar:sonar \
                            -Dsonar.projectKey=divyasri30 \
                            -Dsonar.organization=Divyasri30 \
                            -Dsonar.host.url=https://sonarcloud.io/ \
                            -Dsonar.login=$sonar  """

                    }
                
            }
                
            }

        }
    }
}