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
                sh """
                def records = readJSON file: 'backend/package.json'
                appVersion = packageJson.version
                echo "Version is: $appVersion"
                """
                }
            }
        }
        stage('Test'){
            steps{
                script{
                sh """
                echo "hello,this is Test"
                """
                }
            }
        }
         stage('Deploy'){
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }
            when { 
                environment name: 'DEPLOY_TO', value: 'production'
            }
            steps{
                script{
                sh """
                echo "hello,this is deploy"
                """
                }
            }
        }
         stage('Parallel Stages') {
            parallel {
                stage('STAGE-1') {
                    
                    steps {
                        script{
                            sh """
                                echo "Hello, this is STAGE-1"
                                sleep 15
                            """
                        }
                    }
                }
                stage('STAGE-2') {
                    
                    steps {
                        script{
                            sh """
                                echo "Hello, this is STAGE-2"
                                sleep 15
                            """
                        }
                    }
                }
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


