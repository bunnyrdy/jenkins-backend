pipeline {
    agent { label 'AGENT-1'}
    environment {
        PROJECT = 'EXPENSE'
        COMPONENT = 'BACKEND'
        appVersion = ''
        //ACC_ID = ''
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit:'MINUTES')
    }
    parameters {
        string(name: 'STATEMENT', defaultValue: 'hello; ls /', description: 'What should I say?')
    }
    stages{
        stage('Read Version'){
            steps{
                script{
                def records = readJSON file: 'package.json'
                appVersion = packageJson.version
                echo "Version is: $appVersion"
            }
        }

    }
    post {
            always {
                echo 'i will run if it is success or fail'
                deleteDir()
            }
            failure {
                echo 'i will run when pipeline fails'
            }
            success {
                echo 'i will run when pipeline success'
            }
        }
    }


