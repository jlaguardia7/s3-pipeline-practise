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
                            defaultValue: 's4user',
                            name: 'User',
			    description: 'Required to enter your name',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'DBTag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'UITag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'WEATHERTag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),
                

                            string(
                            defaultValue: 'eric-001',
                            name: 'AUTHTag',
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
cat permission.txt | grep -o $USER
echo $!

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
            agent {
                docker {
                  image 'sonarsource/sonar-scanner-cli:4.7.0'
                }
               }
               environment {
        CI = 'true'
        //  scannerHome = tool 'Sonar'
        scannerHome='/opt/sonar-scanner'
    }
            steps{
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Build-Dev') {
            steps {
                sh '''
                cd UI
                docker build -t devopseasylearning2021/s4-ui:$UITag .
                cd -
                cd DB
                docker build -t devopseasylearning2021/s4-db:$DBTag .
                cd -
                cd auth 
                docker build -t devopseasylearning2021/s4-auth:$AUTHTag .
                cd -
                cd weather 
                docker build -t devopseasylearning2021/s4-weather:$WEATHERTag .
                cd -
                '''

                    }
                }
        stage('Build-Sandbox') {
            steps {
                sh '''
              
                '''

                    }
                }
        stage('Build-Prod') {
            steps {
                sh '''
               
                '''

                    }
                }
        stage('login') {
            steps {
                sh '''
         
                '''

                    }
                }
        stage('Push-to-dockerhub-dev') {
            steps {
                sh '''
       
                '''

                    }
                }
        stage('update helm charts-sanbox') {
            steps {
                sh '''
           
                '''

                    }
                }
        stage('update helm charts-dev') {
            steps {
                sh '''
            
                '''

                    }
                }
        stage('update helm charts-prod') {
            steps {
                sh '''
        
                '''

                            }
                        }
        stage('Wait for argocd') {
            steps {
                sh '''

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
