pipeline {
    agent {
     label ("node1 || node2 ||  node3 || node4 ||  node5 ||  branch ||  main ||  jenkins-node || docker-agent ||  jenkins-docker2 ||  preproduction ||  production")
            }

options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    disableConcurrentBuilds()
    timeout(time: 1, unit: 'HOURS')
    timestamps()
    }  
  
    stages {

        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                        
                        choice(
                            choices: ['Dev', 'Sandbox', 'Prod'], 
                            name: 'Environment'
                                 
                                ),

                          string(
                            defaultValue: 's3user',
                            name: 'User',
			    description: 'Required to enter your name',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'DB-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'UI-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'Weather-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),
                

                            string(
                            defaultValue: 'eric-001',
                            name: 'AUTH-Tag',
			    description: 'Required to enter your image tag',
                            trim: true

                            
                            ),
                        ])
                    ])
                }
            }
        }
    
                
        stage('Permission') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''
        
            }
        }

        stage('Cleaning') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''
                
            }
        }
        stage('Sonarqube') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

            }
        }
        stage('Build-Dev') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('Build-Sandbox') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('Build-Prod') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('login') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('Push-to-dockerhub-dev') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('update helm charts-sanbox') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('update helm charts-dev') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                    }
                }
        stage('update helm charts-prod') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                            }
                        }
        stage('Wait for argocd') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''

                            }
                        }
                    }


post {
   
   success {
      slackSend (channel: '#development-alerts', color: 'good', message: "SUCCESSFUL: Application S4-EKTSS  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

 
    unstable {
      slackSend (channel: '#development-alerts', color: 'warning', message: "UNSTABLE: Application S4-EKTSS  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

    failure {
      slackSend (channel: '#development-alerts', color: '#FF0000', message: "FAILURE: Application S4-EKTSS Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
   
    cleanup {
      deleteDir()
    }
}


                }
