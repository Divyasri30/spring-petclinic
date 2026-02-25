pipeline {
    agent{label 'spec'}
    triggers{
        pollSCM('* * * * *')
    }
    stages {
        stage ('git checkout'){
            steps {
                git url:'https://github.com/Divyasri30/spring-petclinic.git'
                    branch: 'main'
            }
        }
        stage ('buid and scan'){
            steps{
                sh 'mvn package'
            }

        }
    }
}