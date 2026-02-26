pipeline {
    agent{label 'spc'}
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
                 withCredentials([string(credentialsId:'SONAR', variable:' SONAR_TOKEN')]){
                    withSonarQubeEnv('SONAR'){
                        sh """ mvn package sonar:sonar \
                            -Dsonar.projectKey=divyasri30 \
                            -Dsonar.organization=divyasri30 \
                            -Dsonar.host.url=https://sonarcloud.io/ \
                            -Dsonar.login=$SONAR_TOKEN  """

                    }
                
            }
                
            }

        }
    }
}