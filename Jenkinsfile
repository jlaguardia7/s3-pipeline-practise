pipeline {
    agent {
        label ("node1 || node2 ||  node3 || node4 ||  node5 ||  branch ||  main ||  jenkins-node || docker-agent ||  jenkins-docker2 ||  preproduction ||  production")
            }
        environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
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
                            choices: ['DEV', 'SANDBOX', 'PROD'], 
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
    //     stage('Sonarqube') {
    //         agent {
    //             docker {
    //               image 'sonarsource/sonar-scanner-cli:4.7.0'
    //             }
    //            }
    //            environment {
    //     CI = 'true'
    //     //  scannerHome = tool 'Sonar'
    //     scannerHome='/opt/sonar-scanner'
    // }
    //         steps{
    //             withSonarQubeEnv('Sonar') {
    //                 sh "${scannerHome}/bin/sonar-scanner"
    //             }
    //         }
    //     }
        stage('Build-Dev') {
            when{ 
          
                expression {
                  env.Environment == 'DEV' }
          
            }
            
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
            when{ 
          
                expression {
                  env.Environment == 'SANDBOX' }
          
            }
            
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
        stage('Build-Prod') {
            when{ 
          
                expression {
                  env.Environment == 'PROD' }
          
            }

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
        stage('login') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                '''

                    }
                }
        stage('Push-to-dockerhub-dev') {
            when{ 
            
                expression {
                  env.Environment == 'DEV' }
          
            }
            steps {
                sh '''
                docker push devopseasylearning2021/s4-ui:${BUILD_NUMBER}$UITAG
                docker push devopseasylearning2021/s4-db:${BUILD_NUMBER}$DBTAG
                docker push devopseasylearning2021/s4-auth:${BUILD_NUMBER}$AUTHTAH
                docker push devopseasylearning2021/s4-weather:${BUILD_NUMBER}$WEATHERTAG
                '''

                    }
                }
        stage('Push-to-dockerhub-sandbox') {
            when{ 
          
                expression {
                  env.Environment == 'SANDBOX' }
          
            }

            steps {
                sh '''
                docker push devopseasylearning2021/s4-ui:${BUILD_NUMBER}$UITAG
                docker push devopseasylearning2021/s4-db:${BUILD_NUMBER}$DBTAG
                docker push devopseasylearning2021/s4-auth:${BUILD_NUMBER}$AUTHTAH
                docker push devopseasylearning2021/s4-weather:${BUILD_NUMBER}$WEATHERTAG
                '''

                    }
                }
        stage('Push-to-dockerhub-prod') {
            when{ 
          
                expression {
                  env.Environment == 'PROD' }
          
            }

            steps {
                sh '''
                docker push devopseasylearning2021/s4-ui:${BUILD_NUMBER}$UITAG
                docker push devopseasylearning2021/s4-db:${BUILD_NUMBER}$DBTAG
                docker push devopseasylearning2021/s4-auth:${BUILD_NUMBER}$AUTHTAH
                docker push devopseasylearning2021/s4-weather:${BUILD_NUMBER}$WEATHERTAG
                '''

                    }
                }
        stage('update helm charts-dev') {
            when{ 
            
                expression {
                  env.Environment == 'DEV' }
            }
            steps {
	        script {
	          withCredentials([
	            string(credentialsId: 'juan-image', variable: 'TOKEN')
	          ]) {

	            sh '''
                git config --global user.name "jlaguardia7"
                git config --global user.email jlaguard721@gmail.com
                rm -rf s4-pipeline-practise || true
                git clone https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                cd s4-pipeline-practise
        
cat <<EOF > dev-values.yaml           
                image:
                  db:
                     repository: devopseasylearning2021/s4-db
                     tag: "$DBTag"
                  ui:
                     repository: devopseasylearning2021/s4-ui
                     tag: "$UITag"
                  auth:
                     repository: devopseasylearning2021/s4-auth
                     tag: "$AUTHTag"
                  weather:
                     repository: devopseasylearning2021/s4-weather
                     tag: "$WEATHERTag"
EOF
                git add -A
                git commit -m "testing jenkins"
                git push https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                '''

                    }
                }
            }
        }
        stage('update helm charts-sandbox') {
            when{ 
            
                expression {
                  env.Environment == 'SANDBOX' }
            }
            steps {
	        script {
	          withCredentials([
	            string(credentialsId: 'juan-image', variable: 'TOKEN')
	          ]) {

	            sh '''
                git config --global user.name "jlaguardia7"
                git config --global user.email jlaguard721@gmail.com
                rm -rf s4-pipeline-practise || true
                git clone https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                cd s4-pipeline-practise
        
cat <<EOF > sandbox-values.yaml           
                image:
                  db:
                     repository: devopseasylearning2021/s4-db
                     tag: "$DBTag"
                  ui:
                     repository: devopseasylearning2021/s4-ui
                     tag: "$UITag"
                  auth:
                     repository: devopseasylearning2021/s4-auth
                     tag: "$AUTHTag"
                  weather:
                     repository: devopseasylearning2021/s4-weather
                     tag: "$WEATHERTag"
EOF
                git add -A
                git commit -m "testing jenkins"
                git push https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                '''

                    }
                }
            }
        }
        stage('update helm charts-prod') {
            when{ 
            
                expression {
                  env.Environment == 'PROD' }
            }
        
            steps {
	        script {
	          withCredentials([
	            string(credentialsId: 'juan-image', variable: 'TOKEN')
	          ]) {

	            sh '''
                git config --global user.name "jlaguardia7"
                git config --global user.email jlaguard721@gmail.com
                rm -rf s4-pipeline-practise || true
                git clone https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                cd s4-pipeline-practise
        
cat <<EOF > dev-values.yaml           
                image:
                  db:
                     repository: devopseasylearning2021/s4-db
                     tag: "$DBTag"
                  ui:
                     repository: devopseasylearning2021/s4-ui
                     tag: "$UITag"
                  auth:
                     repository: devopseasylearning2021/s4-auth
                     tag: "$AUTHTag"
                  weather:
                     repository: devopseasylearning2021/s4-weather
                     tag: "$WEATHERTag"
EOF
                git add -A
                git commit -m "testing jenkins"
                git push https://jlaguardia7:$TOKEN@github.com/jlaguardia7/s4-pipeline-practise.git
                '''

                    }
                }
            }
        }
        stage('Wait for argocd') {
            when{ 
            
                expression {
                  env.Environment == 'DEV' }
            }
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
